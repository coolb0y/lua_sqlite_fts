```markdown
# lua_sqlite_fts

**lua_sqlite_fts** is a custom build of **SQLite with Full-Text Search (FTS)** exposed to **Lua**, with first-class support for **cross-compilation to ARMv8-A (aarch64)**.

It supports **both static and dynamic builds**, making it suitable for **embedded systems, Android, offline devices, and ARM-based servers**.

## Key Highlights

- SQLite compiled with **FTS3 / FTS4 enabled**
- Lua bindings for SQLite + FTS
- **Cross-compilation for ARMv8-A (aarch64)**
- **Static AND Dynamic builds supported**
- Works for embedded / offline / portable deployments
- CMake-based build system

---

## ARMv8-A Cross-Compilation Support

This project explicitly supports **cross-compiling for ARMv8-A (aarch64)** in **two modes**:

### ✅ Static Build
- Produces a **fully self-contained binary**
- No runtime dependency on system SQLite or Lua
- Ideal for:
  - Embedded Linux devices
  - Offline systems
  - Minimal OS environments

### ✅ Dynamic Build
- Produces a **shared Lua module (.so)**
- Links dynamically against Lua / system libraries
- Ideal for:
  - Android (NDK)
  - ARM servers
  - Environments with existing Lua runtime

Both modes are supported via CMake configuration flags.

---

## Repository Structure

```

lua_sqlite_fts/
├── CMakeLists.txt          # Build & cross-compile configuration
├── lsqlite3.c              # Lua-SQLite binding
├── sqlite3.c               # SQLite amalgamation (FTS enabled)
├── sqlite3.h               # SQLite headers
├── liblua.a                # Lua static library (optional)
├── build/                  # Build output
└── build.instructions      # Build and cross-compile notes

````

---

## Requirements

- CMake ≥ 3.0
- C compiler (gcc / clang)
- Lua headers & library
- ARMv8-A toolchain (for cross-compile)
  - e.g. `aarch64-linux-gnu-gcc`

---

## Building (Native)

```bash
mkdir build
cd build
cmake ..
cmake --build .
````

---

## Cross-Compiling for ARMv8-A (aarch64)

### Toolchain Example

```bash
export CC=aarch64-linux-gnu-gcc
export CXX=aarch64-linux-gnu-g++
```

### Static Build (ARMv8-A)

```bash
cmake .. \
  -DCMAKE_SYSTEM_NAME=Linux \
  -DCMAKE_SYSTEM_PROCESSOR=aarch64 \
  -DBUILD_SHARED_LIBS=OFF

cmake --build .
```

➡ Produces a **fully static Lua-SQLite-FTS binary**.

---

### Dynamic Build (ARMv8-A)

```bash
cmake .. \
  -DCMAKE_SYSTEM_NAME=Linux \
  -DCMAKE_SYSTEM_PROCESSOR=aarch64 \
  -DBUILD_SHARED_LIBS=ON

cmake --build .
```

➡ Produces a **shared `.so` Lua module** for ARMv8-A.

---

## Usage (Lua)

```lua
local sqlite = require("lsqlite3_fts")

local db = sqlite.open("test.db")

db:exec([[
  CREATE VIRTUAL TABLE docs USING fts4(content);
]])

db:exec("INSERT INTO docs VALUES ('Lua SQLite FTS on ARMv8-A')")

for row in db:nrows("SELECT rowid, content FROM docs WHERE docs MATCH 'ARM'") do
  print(row.rowid, row.content)
end

db:close()
```

---

## Full-Text Search (FTS)

* Uses SQLite **FTS3 / FTS4**
* Supports `MATCH`, tokenization, ranking
* Optimized for **offline search** use cases

Example:

```sql
SELECT * FROM docs WHERE docs MATCH 'search term';
```

---

## Use Cases

* Offline search engines
* Embedded ARM devices
* Android NDK Lua modules
* Portable search runtimes
* ARM servers without SQLite FTS enabled

---

## License

* **SQLite**: Public Domain
* **Lua**: MIT License
* Project glue code follows repository license

---

## Notes

* FTS5 can be enabled with minor build flag changes if required
* Designed to avoid runtime SQLite dependencies in static mode
* Suitable for Lua 5.x and LuaJIT (depending on build)

```

If you want, I can also:
- Add **Android NDK example**
- Add **LuaJIT-specific notes**
- Add **CI cross-build badges**
- Tighten wording for **GitHub visibility / SEO**

Tell me what you want next.
```
