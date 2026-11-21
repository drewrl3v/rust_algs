# rust_algs

## Install Prerequisites

You need to have **Rust** installed. It comes with `cargo`, which is used to install `mdBook` and plugins.

Install Rust on MacOS and Linux Users:

    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    rustup update

Windows Users:

    Download and run the installer: https://rustup.rs

## Note:

mdbook and mdbook-katex versions needed to run this book:

    cargo install mdbook --version 0.4.52
    cargo install mdbook-katex --version 0.10.0


## Serving The Book:

You can serve the book by running the following command in the `book` directory
	
		mdbook serve
