A module can be a file alongside the main or lib.rs file, or inside folders
```
my_project/
├── src/
│   ├── main.rs
│   ├── lib.rs
│   ├── module1.rs
│   ├── module2.rs
│   └── utils/
│       ├── mod.rs
│       ├── helper.rs
│       └── constants.rs
├── Cargo.toml
└── README.md
```

inside the src/utils/mod.rs
```rust
// In src/utils/mod.rs

// Import sub-modules
mod helper;
mod constants;

// Re-export sub-modules
pub use helper::*;
pub use constants::*;
```

if i want to access something inside helper from outside
```rust
use crate::utils::helper::helper_func;
```

modules can also be inside other files after the keyword `mod` inside spec

```rust
mod my_mod {
	code
}
```

modules