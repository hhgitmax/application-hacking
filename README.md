# Analyzing of a binary

One can get a binary by initiating a project with

`cargo new hacking-report`

Then going to the project folder `cd hacking-report`
And compiling the project with `cargo b --release`
Out of that we get a binary
<img width="1916" height="846" alt="image" src="https://github.com/user-attachments/assets/c1378dd9-9d9e-4f75-9eef-72a5d65f1b2b" />

By default some debuginfo is attached, which can be seen with `du -sh target/release/hacking-report`
But it can be stripped with the `strip` command
<img width="1098" height="194" alt="image" src="https://github.com/user-attachments/assets/5a7e3317-66f8-4ba6-be3e-c0ab51624910" />
As a result of the action, we loose debuginfo that is usefult with backtraces and `gdb` debugging

First what we should do is to analyze the binary that we got with `file`
<img width="974" height="72" alt="image" src="https://github.com/user-attachments/assets/1bcc1adf-a2dc-4c30-ab03-c69189425a1d" />

We can also analyze a dynamically linked libraries with `otool` or `ldd` on linux. This is how we know which libraries we can preload to attack.
<img width="1282" height="116" alt="image" src="https://github.com/user-attachments/assets/5c03e960-141d-4ca7-bf72-a607cb9a26a4" />

From the output we can see the arch, which is the most important for our purposes. This way we know which tools to use and where we can run it.

We can analyze the binary with `strings` to find our "Hello world" string from there as well as some standart rust panicking strings
<img width="1030" height="798" alt="image" src="https://github.com/user-attachments/assets/6de62ccf-b75e-411c-876d-03f901a983fc" />

This can be changed by specifying

```toml
[profile.release]
panic = "abort"
```
in Cargo.toml

<img width="1022" height="794" alt="image" src="https://github.com/user-attachments/assets/edc069b5-ad29-4e60-8245-3bd14f3bc2af" />

Now that we are sure we can continue to dymanic analysis. We can set a breakpoint on main, so we don't run it, but first look at the assembly.

<img width="1432" height="1172" alt="image" src="https://github.com/user-attachments/assets/e0fdc3fa-12fe-4151-b311-8d4478b6361f" />

Now that we're sure it's just a hello world, we can run it with `c`

<img width="766" height="160" alt="image" src="https://github.com/user-attachments/assets/801230fe-7b31-4242-8930-1b224e9b0091" />




