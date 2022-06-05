---
layout: post
title: "Exploring Rust RE pt. 1"
---

## Introduction

Let's take a look at what Rust looks like in Ghidra.

## Println!

The `println` command is actually a macro. It's a macro so you can use format paramaters. Format parameters in C/C++ are implemented unsing the concept of variable arguments or (`varargs`). This means that there is a dynamic number of arguments passed on the stack to the function being called. Rust doesn't like this concept, so to support 
using formats they need to make println! a macro. Lets take a look at why.

### No Format

The first example has the following code:

```rust
fn main() {
	println!("Hello, world!");
}
```

If this were C, our disassembly would look pretty simple. We'd see a single function call passing a pointer to a string from `.rodata`. 
The function that would be called would be `puts(...)` regardless of whether or not the developer tried to use `printf(...)` or not. 
This is a common protection built into many compilers to help prevent string format 
vulnerabilities.

It might look something like this:

```c
void main(void)
{
  puts(PTR_s_Hello,_world!_001401234);
}
```



Instead we get the following (Coppied from Ghidra's Disassembly):

```c
void main::main(void)

{
  undefined local_30 [48];
  
  core::fmt::Arguments::new_v1(local_30,&PTR_s_Hello,_world!_00140568,1,&DAT_00134010,0);
  std::io::stdio::_print(local_30);
  return;
}
```

Before we continue it might be interesting for the reader to note, that though we are looking strictly at the decompilation at this time, the disassembly isn't all
that intersting, at least not on my x86_64 machine. Function calls are made with the exact same calling convention defined by the x86_64 ABI. Nothing too wild...

As you can see there are some things which are clearly different. First, we have this concept of `core::fmt::Arguments::new_v1`, which takes a whopping 5 parameters.

In our code above `local_30` is what we might traditionally think of as an object, but in this case allocated on the stack. `&PTR_s_Hello,_world!_00240568` is obviously
our string in .rodata. We don't have much of a clue as to what the other arguments are. So before we go any further, lets run another test where we actually provide 
a format string.

```rust
fn main() {
  let name = String::from("CyberSmash");
  println!("Hello, World: {}", name);
}        
```

This code now adds a format string variable to the mix. If we analyze it in ghidra we get the following:

```c
void main::main(void)

{
  undefined local_70 [24];
  undefined local_58 [48];
  undefined local_28 [16];
  undefined *local_18;
  
  <alloc::string::String_as_core::convert::From<&str>>::from
            (local_70,"CyberSmashHello, World: \ncalled `Result::unwrap()` on an `Err` value",10);
  local_18 = local_70;
                    /* try { // try from 0010886c to 001088be has its CatchHandler @ 001088cd */
  local_28 = core::fmt::ArgumentV1::new(local_18,<alloc::string::String_as_core::fmt::Display>::fmt)
  ;
  core::fmt::Arguments::new_v1
            (local_58,&PTR_s_Hello,_World:_called_`Result_un_001434e8,2,local_28,1);
                    /* try { // try from 001088e0 to 001088ed has its CatchHandler @ 001088cd */
  std::io::stdio::_print();
  core::ptr::drop_in_place<alloc::string::String>(local_70);
  return;
}

```
