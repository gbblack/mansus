---
tags:
  - type/book_chapter
created:
---
[[The Rust Programming Language]]
# **Error Handling**

> [!abstract] Summary
### **Note**
---
##### **Unrecoverable Errors with `panic!`**
##### **Recoverable Errors with Result**
**Matching on Different Errors**
**Propagating Errors**
##### **To `panic!` or Not to `panic!`**
**Examples, Prototype Code, and Tests**
**Cases In Which You Have More Information Than the Compiler**
**Guidelines for Eror Handling**
**Creating Custom Types for Validation**
### **Highlights**
---
9 (page number)
> example highlight
##### **Citation**
---
```
First Name Initial. Last Name, "Chapter Title" in *Book Title*, edition. First Name Initial. Last Name (for all editors), Ed(s). City, (US State Only), Country: Publication, Year, pp. start page-end page.
```

> [!note] Nota Bene

---
##### Completion Checklist
###### I. To Become Dark
- [ ] Write the Chapter title in the heading.
- [ ] Fill in the `created` property.
- [ ] Link to the book's index note.
- [ ] Complete the `Citation` section.
- [ ] Read the chapter once in its entirety with focus, no music.
- [ ] Add tag `status/dark`.
###### II. From Dark to Dawn
- [ ] Read the chapter again, this time copy pasting interesting sections into the note under `Highlights`. Include the page number right above the block.
- [ ] **Bold** the portions of the `Highlights` you find most interesting.
- [ ] ==Highlight== the best parts of the bolded sections.
- [ ] Update status tag to `status/dawn`.
###### III. From Dawn to Day
- [ ] Write the chapter `Summary`.
- [ ] Remove or complete the `Nota Bene` as necessary.
- [ ] Fill in the context tags for the metadata.
- [ ] Update status tag to `status/day`.
- [ ] Remove this checklist.

**For a technical text:**
###### II. From Dark to Dawn
- [ ] Wait at least 5mins before beginning this section.
- [ ] In the `Note` section breakdown the chapter into its subheading: all the sections in bold.
- [ ] In bullet points, under each section and sub section heading summarise the major points of that section. In these bullet point make links to anything that could be referenced in a permanent note. This will take awhile.
- [ ] While summating the sections copy paste interesting sections into the note under `Highlights`. Include the page number right above the block. These highlights should only be the author's own reflections that you think are interesting, nothing definitive. There may be nothing.
- [ ] Update status tag to `status/dawn`.
###### III. From Dawn to Day
- [ ]  **Bold** the portions of the `Highlights` you find most interesting.
- [ ] ==Highlight== the best parts of the bolded sections.
- [ ] Write the chapter `Summary`.
- [ ] Remove or complete the `Nota Bene` as necessary.
- [ ] Fill in the context tags for the metadata.
- [ ] Update status tag to `status/day`.
- [ ] Remove this checklist.

In rust there are two types or errors: *recoverable* and *unrecoverable*. To handle these two errors Rust does not use exceptions it instead uses the type `Result<T, E>` for recoverable errors and `panic!` [[macros]] for unrecoverable errors. 

### Unrecoverable Errors

There are two ways to cause a `panic!` error. The first is by taking an action that causes the code to panic, such as accessing an array index that is out of bounds, or by explicitly calling `panic!`. The default behaviour of panic is to print a failure message, clean up the stack and quit. You can also config through environment variables were Rust will display the call stack to better report the bug.

> [!info] Unwinding or aborting in response to a panic
> When the Rust cleans up the stack when a panic occurs this is called **unwinding**. This occurs by default but can take time and be a lot of work. Rust gives you the option to *abort* it instead which ends the program without cleaning it.
> Any memory that the program was using will then need to be cleaned up by the OS. To switch from the default behaviour to aborting you add `panic = 'abort'` to the appropriate `[profile]` sections in the *Cargo.toml* file. This can be done on a mode by mode basis

To call panic directly we do something like this:

```rust
fn main() {
    panic!("crash and burn");
}
```

Mostly though we will be encountering panic when it is triggered in someone else's code. To backtrace the call stack and see exactly where and what failed we can call `RUST_BACKTRACE` after the program quits: `RUST_BACKTRACE=1 cargo run`. The only requirement is that it is set to anything that is not 0.