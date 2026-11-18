# TianoCore EDK II Documentation

This repository contains the documentation for TianoCore EDK II, structured as an mdBook.

## Building the Documentation

### Prerequisites

| Tool | Required Version |
|------|------------------|
| mdbook | 0.4.52 |
| mdbook-admonish | 1.20.0 |
| mdbook-mermaid | 0.15.0 |
| mdbook-linkcheck | 0.7.7 |

Install mdBook and dependencies:

```bash
cargo install mdbook@0.4.52 mdbook-admonish@1.20.0 mdbook-mermaid@0.15.0 mdbook-linkcheck@0.7.7
```

Or download pre-built binaries from the associated release pages:
- [mdBook](https://github.com/rust-lang/mdBook/releases)
- [mdBook-admonish](https://github.com/tommilligan/mdbook-admonish/releases)
- [mdBook-mermaid](https://github.com/badboy/mdbook-mermaid/releases)
- [mdBook-linkcheck](https://github.com/Michael-F-Bryan/mdbook-linkcheck/releases)

### Build and Serve

To build and serve the documentation locally:

```bash
mdbook serve
```

Then open your browser to `http://localhost:3000`

To build the documentation without serving:

```bash
mdbook build
```

The output will be in the `book/` directory.

### Clean Build

To clean the build directory:

```bash
mdbook clean
```

## Repository Structure

- `SUMMARY.md` - Table of contents for the book
- `book.toml` - mdBook configuration
- `index.md` - Introduction/landing page
- Content organized in thematic directories:
  - `build-tooling/` - Build system documentation
  - `community/` - Community information and support
  - `development/` - Development tutorials and guides
  - `governance/` - Project governance and policies
  - `platforms-packages/` - Platform and package documentation
  - `projects/` - Additional projects
  - `reference/` - Specifications, standards, and references
  - `releases-history/` - Release information
  - `security/` - Security processes and advisories
  - `archives/` - Archived historical content

## Contributing

Please see our [contribution guidelines](development/contribution-guides/how_to_contribute.md).

## License

See [LICENSE](LICENSE) file for details.
