---
tags:
  - type/book_chapter
created:
---
[[The Rust Programming Language]]
# **More about Cargo and Crates.io**

> [!abstract] Summary
### **Note**
---
##### **Customizing Builds with Release Profiles**
##### **Cargo Workspace**
##### **Installing Binaries from Crates.io with `cargo install`**
##### **Extending Cargo with Custom Commands**
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
Complete documentation: [cargo](https://doc.rust-lang.org/cargo/)
##### Customizing Builds with Release Profiles

In Rust [[release profiles]] are predefined and customizable profiles with different configurations that allow a programmer to have more control over various options compiling code. Each profile is independently configured.

Cargo has two main profiles: the `dev` profile for when `cargo build` is run and the `release` profile when you run `cargo build --release`. The `dev` profile has good defaults for development and the `release` profile has good defaults for release builds.

If you want to change or update these default configurations we can do that by updating the `[profile.*]` section in the *Cargo.toml* file. This way we can override to add any values to the default. 

```rust
[profile.dev]
opt-level = 0

[profile.release]
opt-level = 3
```

The `opt-level` setting controls the number of optimizations Rust applies, ranged form 0 to 3. More optimizations makes for a longer compile time so the default `dev` is set to 0 to keep it form slowing down. For `release` that value is 3. To override this setting just change the value. 