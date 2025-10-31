## File Preview Strategy and Logic in Superfile

  The file preview feature in Superfile uses a sophisticated multi-layered approach to display different types of files:

  1. File Type Detection and Routing

  The preview system (src/internal/ui/preview/model.go:243-278) determines how to render a file based on:
  - File metadata: Uses os.Stat() to check if the path is a directory or regular file
  - File extension: Checks against a list of unsupported formats
  - Content type: Distinguishes between images and text files

  2. Directory Preview

  For directories (renderDirectoryPreview):
  - Lists directory contents up to the preview height limit
  - Sorts entries with directories first, then alphabetically
  - Shows file/folder icons with appropriate colors using Nerdfont icons
  - Displays "Empty" message for empty directories

  3. Image Preview

  The image preview system (src/pkg/file_preview/image_preview.go) supports two rendering methods:

  Kitty Graphics Protocol (Preferred)

  - Detects terminal support for Kitty protocol (kitty, ghostty, WezTerm, iTerm2, etc.)
  - Uses rasterm library to render images using escape sequences
  - Maintains aspect ratio and fits within terminal cell dimensions
  - Clears previous images before rendering new ones
  - Falls back to ANSI if Kitty fails

  ANSI Rendering (Fallback)

  - Converts images to colored Unicode blocks (▄ character)
  - Each terminal row represents 2 pixel rows
  - Uses color caching for performance
  - Resizes images to fit terminal dimensions

  Image Processing Pipeline:

  1. Format support: JPEG, PNG, GIF, WebP, BMP, TIFF, SVG, ICO
  2. EXIF orientation: Automatically corrects image rotation based on EXIF data
  3. Resolution limiting: Caps images at 1080p (1920x1080) for performance
  4. Caching: Implements LRU cache with 5-minute expiration for rendered previews

  5. Text File Preview

  Text file rendering (renderTextPreview) includes:

  Syntax Highlighting:

  - Uses chroma lexer to detect file format
  - Falls back to plain text for unrecognized formats
  - Checks if file is actually text using binary detection

  Code Previewers:

  1. Bat (if installed):
    - External command with 500ms timeout
    - Supports syntax highlighting with themes
    - Line limiting for performance
  2. Ansichroma (built-in fallback):
    - Uses Go library for syntax highlighting
    - Respects terminal theme settings

  Text Processing:

  - Reads files with UTF-8 BOM support
  - Line truncation based on terminal width
  - Preview line limiting for large files

  5. Performance Optimizations

  - Asynchronous loading: Preview generation runs in background (getFilePreviewCmd)
  - Request counting: Prevents duplicate preview requests
  - Caching: Both image renders and file content are cached
  - Progressive loading: Shows "Loading..." while generating preview
  - Size limits: 100MB max file size for images

  6. Configuration Options

  Users can control preview behavior through config.toml:
  - default_open_file_preview: Auto-open preview panel
  - show_image_preview: Enable/disable image previews
  - code_previewer: Choose between 'bat' or built-in highlighter
  - file_preview_width: Configure preview panel width
  - transparent_background: Control background rendering

  7. Special Handling

  - Non-regular files: Skips preview for special files (devices, pipes, etc.)
  - Unsupported formats: Shows appropriate error messages
  - Empty files: Displays "Empty" message
  - Errors: Graceful degradation with user-friendly error messages

  The preview system is designed to be fast, flexible, and user-friendly, with multiple fallback options to ensure content is always viewable regardless of
  terminal capabilities or file types.
  
## Learning Path for Terminal Preview Rendering

  1. Start with Terminal Fundamentals

  ANSI Escape Sequences
  - Research ANSI/VT100 terminal standards
  - Learn about escape codes for colors, cursor movement
  - Resources:
    - "ANSI escape code" Wikipedia page
    - XTerm Control Sequences documentation
    - Try simple experiments: echo -e "\033[31mRed Text\033[0m"

  Terminal Capabilities
  - Learn about terminfo/termcap databases
  - Understand how terminals report capabilities
  - Tools: tput, infocmp, environment variables like $TERM

  2. Understand the Constraints

  Character Grid System
  - Terminals display fixed-width character grids
  - Each cell has foreground/background colors
  - Limited to Unicode characters available in fonts

  Performance Limitations
  - Sending data over stdout is slow
  - Terminal emulators have rendering bottlenecks
  - Need to minimize escape sequences

  3. Discover Image-in-Terminal Techniques

  Block Characters Method (Most Basic)
  ▀▄█▌▐░▒▓
  - Unicode has various block characters
  - Half blocks (▀▄) double vertical resolution
  - Quarter blocks exist in some fonts

  Research Papers & Standards
  - Search for "Sixel graphics" (DEC's 1980s standard)
  - iTerm2 Inline Images Protocol documentation
  - Kitty Graphics Protocol specification
  - Terminal WG (Terminal Working Group) discussions

  4. Learn from Protocol Documentation

  Kitty Graphics Protocol
  # Kitty's documentation explains the escape sequence format
  # Example: ESC_Gf=24,s=100,v=100,a=T,m=1;BASE64_DATA\ESC

  iTerm2 Protocol
  # iTerm2 uses OSC sequences
  # ESC]1337;File=inline=1:BASE64_DATA^G

  5. Study Terminal Emulator Source Code

  Look at how terminals implement these features:
  - Kitty (Python/C) - has excellent documentation
  - Alacritty (Rust) - clean modern codebase
  - Terminal.app, iTerm2 - for macOS specifics

  6. Experiment with Detection Methods

  Terminal Detection Strategies
  # Check environment variables
  echo $TERM_PROGRAM  # "iTerm.app", "kitty", etc.
  echo $TERM          # "xterm-256color", "xterm-kitty"

  # Query terminal capabilities
  echo -e '\e[c'      # Request device attributes

  7. Learn Image Processing Basics

  Downsampling Algorithms
  - Nearest neighbor
  - Bilinear interpolation
  - Lanczos resampling

  Color Quantization
  - Converting 24-bit color to terminal's 256 colors
  - Dithering techniques (Floyd-Steinberg)

  8. Performance Optimization Patterns

  Common Strategies
  - Caching rendered output
  - Async rendering in background
  - Progressive enhancement (show placeholder → full preview)
  - Diff-based updates (only send changes)

  9. Resources to Find These Techniques

  Where to Look
  10. Academic Papers: Search for "terminal graphics", "console visualization"
  11. Unix History: Understand how problems were solved historically
  12. Terminal Emulator Forums: Discussions about new features
  13. Open Source Projects:
    - w3m (terminal web browser with images)
    - ranger (file manager with previews)
    - viu (terminal image viewer)
    - chafa (terminal graphics library)

  Communities
  - r/commandline on Reddit
  - Terminal emulator GitHub issues/discussions
  - Hacker News threads about terminal tools

  10. Build Your Mental Model

  Key Questions to Research
  11. "How do I detect what my terminal supports?"
  12. "What's the fastest way to update the screen?"
  13. "How do I handle different terminal sizes?"
  14. "What fallbacks should I implement?"
  15. "How do existing tools solve this?"

  16. Practical Learning Path

  17. Start Simple: Build a basic ANSI color art display
  18. Add Images: Implement ASCII art conversion
  19. Try Blocks: Use Unicode blocks for better resolution
  20. Learn Protocols: Implement Sixel or Kitty support
  21. Add Detection: Auto-detect terminal capabilities
  22. Optimize: Add caching, async rendering
  23. Polish: Handle edge cases, errors gracefully

  24. Modern Best Practices (What you'd discover)

  - Progressive Enhancement: Basic → ANSI → Sixel → Kitty
  - Feature Detection: Don't rely on terminal names alone
  - Fallback Chains: Multiple rendering methods
  - Respect User Settings: Some users disable images
  - Performance First: Terminal I/O is expensive

  The key insight is that terminal rendering is about working within constraints creatively. Each technique (ANSI blocks, Sixel, Kitty protocol) emerged from
   people pushing these boundaries. By understanding the fundamentals and researching how others solved similar problems, you'd naturally arrive at similar
  solutions.
## Make cannon
Building an E-Reader with Go and Bubbletea: Learning Strategy

  1. Understand the Domain First

  E-Book Formats (Start here - you need to know what you're dealing with)
  - EPUB: Most common, essentially a ZIP file with HTML/CSS
    - Learn: EPUB 3 specification from IDPF/W3C
    - Key insight: It's structured XHTML with metadata
  - MOBI/AZW: Amazon's format (more complex)
  - PDF: Fixed layout (very different from reflowable text)
  - Plain text/Markdown: Simplest starting point

  E-Reader Core Concepts
  - Reflowable vs fixed layout
  - Pagination vs scrolling
  - Text justification and hyphenation
  - Reading progress tracking
  - Bookmarks and annotations

  2. Study How Superfile's Architecture Applies

  Relevant Patterns from Superfile
  - Model-View separation: Preview panel pattern
  - File handling: How it reads different file types
  - Rendering pipeline: Text processing → formatting → display
  - Async operations: Loading files in background
  - Configuration system: User preferences

  Key Differences to Consider
  - E-reader focuses on single file, long-form reading
  - Need pagination instead of file listing
  - Text reflow is critical vs fixed-width preview

  3. Essential Go Libraries to Research

  EPUB Parsing
  - github.com/taylorskalyo/goreader/epub - Simple EPUB reader
  - github.com/bmaupin/go-epub - EPUB creation/parsing
  - Study their source to understand EPUB structure

  Text Processing
  - github.com/mattn/go-runewidth - Unicode width handling
  - github.com/muesli/reflow - Text wrapping/justification
  - golang.org/x/text - Unicode normalization

  Terminal UI Beyond Bubbletea
  - github.com/charmbracelet/lipgloss - Styling (already in Superfile)
  - github.com/charmbracelet/bubbles - Pre-built components
  - github.com/muesli/termenv - Terminal capability detection

  4. Learning Path: Domain Knowledge

  Week 1-2: E-Book Formats
  5. Download sample EPUBs from Project Gutenberg
  6. Unzip them and explore structure
  7. Read EPUB 3 Overview specification
  8. Learn about OPF (package file) and NCX (navigation)

  Week 3-4: Text Rendering Concepts
  9. Research text justification algorithms
  10. Understand hyphenation rules and libraries
  11. Learn about font metrics and line height
  12. Study Unicode text segmentation (word/line breaks)

  Week 5-6: Reader UX Patterns
  13. Use existing e-readers (Calibre, Apple Books, Kindle)
  14. Note their navigation patterns
  15. Study reading progress calculation
  16. Understand bookmark/annotation systems

  17. Architecture Planning Based on Superfile

  Adapt Superfile's Patterns
  Superfile Pattern → E-Reader Adaptation
  -----------------------------------------
  File Preview     → Page/Chapter Display
  File List        → Table of Contents
  Metadata Panel   → Book Info/Progress
  Config System    → Reading Preferences
  Async Loading    → Chapter Pre-fetching

  Key Architectural Decisions
  18. Text Storage: In-memory vs streaming
  19. Pagination: Pre-calculate vs dynamic
  20. Rendering: Full redraw vs differential updates
  21. State Management: Reading position, bookmarks

  22. Technical Challenges to Research

  Text Reflow Problem
  - Research: "Knuth-Plass line breaking algorithm"
  - Simpler: "Greedy line breaking algorithm"
  - Consider: Terminal width changes

  Pagination Strategies
  1. Character-based: Split at N characters
  2. Line-based: Split at N lines
  3. Viewport-based: Calculate what fits on screen

  Performance Considerations
  - Large file handling (some EPUBs are 10MB+)
  - Smooth scrolling/paging
  - Memory usage for book content

  7. Bubbletea-Specific Learning

  Study Bubbletea Examples
  8. Start with Bubbletea tutorials
  9. Look at bubbles viewport component
  10. Study pager example in Bubbletea repo
  11. Understand Update/View/Init pattern

  Key Components You'll Need
  - Viewport for text display
  - List for table of contents
  - Input for search/goto
  - Progress bar for reading position

  8. Resources for Deep Learning

  E-Book Specifications
  - IDPF EPUB 3 Specification
  - EPUB Samples GitHub repo
  - Calibre source code (Python, but great reference)

  Text Rendering
  - "Digital Typography" by Donald Knuth
  - Unicode Line Breaking Algorithm (UAX #14)
  - Browser text rendering articles (similar problems)

  Terminal UI Design
  - Charm.sh blog posts
  - Bubbletea example repository
  - Terminal UI best practices guides

  9. Incremental Development Strategy

  Phase 1: Plain Text Reader (2 weeks)
  - Load .txt files
  - Basic pagination
  - Simple navigation (j/k, PgUp/PgDn)

  Phase 2: EPUB Support (3 weeks)
  - Parse EPUB structure
  - Display single chapter
  - Navigate between chapters

  Phase 3: Reading Features (2 weeks)
  - Bookmarks
  - Reading progress
  - Search functionality

  Phase 4: Polish (2 weeks)
  - Configurable fonts/colors
  - Better text justification
  - Performance optimization

  10. Common Pitfalls to Avoid

  From Terminal Rendering
  - Don't assume terminal dimensions are stable
  - Handle Unicode properly (emoji, CJK characters)
  - Test on different terminals

  From E-Book Domain
  - EPUBs can have invalid HTML
  - Not all EPUBs follow the spec
  - DRM-protected files exist

  11. Where to Find Help

  Communities
  - r/golang for Go-specific questions
  - MobileRead Forums for e-book format knowledge
  - Charm Discord for Bubbletea help
  - Standard Ebooks for well-formatted EPUBs

  Code to Study
  - epr - Python terminal EPUB reader
  - epy - Another Python terminal reader
  - termpub - Rust terminal EPUB reader
  - Calibre's e-book parsing libraries

  12. Building Your Mental Model

  Think of an e-reader as:
  13. Parser Layer: Extracts text from container format
  14. Layout Engine: Breaks text into pages/screens
  15. Render Layer: Displays formatted text
  16. Navigation Layer: Handles user input
  17. State Layer: Tracks reading progress

  Each layer has well-established patterns and algorithms you can research independently before combining them.

  The key insight from studying Superfile is that complex TUI applications are really about:
  - Smart data structures (model)
  - Efficient rendering (view)
  - Responsive event handling (update)
  - Good abstractions between layers
  
