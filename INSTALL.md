# Installation Guide: JDK, sbt, and VS Code

This guide walks you through setting up everything you need for the Scala
programming assignments on **Windows**, **macOS**, and **Linux**.

- [Requirements](#requirements)
- [Windows](#windows)
- [macOS](#macos)
- [Linux](#linux)
- [Verifying Your Installation](#verifying-your-installation)
- [Scala REPL (Optional)](#scala-repl-optional)
- [Setting Up VS Code](#setting-up-vs-code)
- [Troubleshooting](#troubleshooting)

## Requirements

You only need **two** things. Everything else is handled for you.

| Tool | What it is for | Required? |
| :-- | :-- | :-- |
| **JDK 21** (or 17) | runs Scala and sbt | **Yes** |
| **sbt** (any recent version) | builds and tests the assignments | **Yes** |
| **Scala** (`scala` command) | the REPL, for trying code by hand | No |

> [!IMPORTANT]
>
> You do **not** need to install Scala itself. Each assignment pins
> **Scala 3.3.3** and **sbt 1.9.9** in its build files, and sbt downloads them
> for you.
>
> Install Scala only if you want the **REPL** to try code by hand --
> see [Scala REPL (Optional)](#scala-repl-optional).

> [!WARNING]
>
> Do **NOT** install **JDK 25 or later**.
>
> Scala 3.3.3 runs on JDK 21 at the newest. JDK 25 requires Scala 3.3.7+ and
> JDK 26 requires Scala 3.3.8+, so the assignments will fail to compile on
> those. This is the single most common setup mistake -- pay attention to the
> version when you install.

> [!NOTE]
>
> You may install any recent sbt, including sbt 2.x. The sbt launcher reads
> `sbt.version` from each project's `project/build.properties` and
> automatically uses **sbt 1.9.9** for the assignments.

## Windows

Use **winget**, which is already included in Windows 10 and 11. Open
**PowerShell** and run:

```powershell
winget install EclipseAdoptium.Temurin.21.JDK
winget install sbt.sbt
```

Then **close and reopen PowerShell** so that the updated `PATH` takes effect.

> [!NOTE]
>
> If `winget` is not recognized, install **App Installer** from the Microsoft
> Store, or download the installers manually instead:
>
> * JDK: <https://adoptium.net/temurin/releases/?version=21> -- choose the
>   `.msi` package for Windows
> * sbt: <https://www.scala-sbt.org/download/> -- choose the `.msi` package

## macOS

Use [Homebrew](https://brew.sh/):

```bash
brew install --cask temurin@21
brew install sbt
```

> [!WARNING]
>
> Install `temurin@21`, **not** plain `temurin`. The unversioned cask currently
> installs **JDK 26**, which the assignments do not support.

If you do not use Homebrew, download the `.pkg` installer from
<https://adoptium.net/temurin/releases/?version=21> and install sbt from
<https://www.scala-sbt.org/download/>.

## Linux

The easiest way is [SDKMAN](https://sdkman.io/):

```bash
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"

sdk list java            # find the latest 21.x.y-tem identifier
sdk install java 21.0.9-tem   # replace with the identifier you found
sdk install sbt
```

> [!NOTE]
>
> Patch versions change over time, so run `sdk list java` and pick the newest
> entry ending in **`-tem`** (Eclipse Temurin) whose version starts with `21`.

Alternatively, install the `.deb` / `.rpm` packages from
<https://adoptium.net/temurin/releases/?version=21>.

> [!TIP]
>
> SDKMAN also works on **macOS**, and on **Windows** only inside **WSL** or
> **MSYS+MinGW** -- it requires a `bash` environment and cannot be installed
> natively on Windows.

## Verifying Your Installation

Open a **new** terminal and run:

```bash
java -version
sbt --version
```

`java -version` must report version **21** (or 17):

```
openjdk version "21.0.9" 2026-XX-XX LTS
```

Then move into any assignment directory and run the tests:

```bash
cd scala-tutorial
sbt test
```

The first run downloads Scala 3.3.3, sbt 1.9.9, and the libraries, so it can
take several minutes. Later runs are much faster.

## Scala REPL (Optional)

The **REPL** (Read-Eval-Print-Loop) lets you evaluate Scala expressions
interactively. It is useful for trying things out during lectures, but it is
**not needed for the assignments**.

### Option 1: On the web, with nothing to install

Use the Scala Playground: <https://scastie.scala-lang.org/>

### Option 2: A standalone `scala` command

This needs a **JDK**, so install one first by following
[Windows](#windows), [macOS](#macos), or [Linux](#linux) above. Then install
Scala itself:

* **macOS:** `brew install scala`
* **Linux:** `sdk install scala`
* **Windows:** install [Coursier](https://get-coursier.io/docs/cli-installation)
  and run `cs setup`

Check that it works with:

```bash
scala -version
```

> [!WARNING]
>
> On Windows, do **not** use `winget install Scala.Scala` -- that package only
> provides **Scala 2**, not Scala 3.

> [!NOTE]
>
> The Scala version you get here may be newer than the **3.3.3** used by the
> assignments. That is fine for trying things out in the REPL, but always build
> and test the assignments with `sbt`, which uses 3.3.3.

## Setting Up VS Code

[VS Code](https://code.visualstudio.com/) with the **Metals** extension is the
recommended editor for these assignments.

### 1. Install the extension

Install **Metals** (publisher `scalameta`, extension ID `scalameta.metals`)
from the Extensions panel, or from the
[Marketplace](https://marketplace.visualstudio.com/items?itemName=scalameta.metals).

> [!WARNING]
>
> Do **not** install the old *"Scala Language Server"* or *"Scala (sbt)"*
> extensions alongside Metals. Multiple Scala language servers conflict with
> each other.

### 2. Open the assignment folder as the project root

Open the folder that **directly contains `build.sbt`**:

```
scala-tutorial          <-- open THIS folder
├─ build.sbt
├─ project/
└─ src/
```

> [!WARNING]
>
> This is the most common VS Code mistake. If you open a parent folder (or a
> single `.scala` file), Metals cannot find the build and you get no
> completions or error highlighting.

### 3. Import the build

Metals shows an **Import build** notification the first time you open the
project. Click it.

If you missed the notification, run it from the Command Palette
(`Ctrl+Shift+P`, or `Cmd+Shift+P` on macOS):

```
Metals: Import build
```

The first import can take several minutes. Watch the progress in the status
bar; wait for it to finish before expecting completions.

> [!NOTE]
>
> Metals uses its own build server, so importing works even without a global
> sbt installation. You still want sbt installed to run `sbt test` from the
> terminal.

### 4. Useful commands and features

| Action | How |
| :-- | :-- |
| Run tests for the current file | `Metals: Test Current File` |
| Run the current file | `Metals: Run Current File` |
| Re-import after editing `build.sbt` | `Metals: Import build` |
| Diagnose a broken setup | `Metals: Run Doctor` |
| Jump to a definition | `F12` |
| Rename a symbol | `F2` |

**Metals: Run Doctor** is the first thing to try whenever the editor behaves
oddly -- it reports which build targets and JDK Metals actually picked up.

### 5. Optional: pin the JDK for Metals

If you have several JDKs installed and Metals picks the wrong one, set it
explicitly in your workspace `.vscode/settings.json`:

```json
{
  "metals.javaHome": "/path/to/jdk-21"
}
```

Find the path with `java -XshowSettings:properties -version 2>&1 | grep java.home`
(on Windows, `where java`).

## Troubleshooting

### `sbt: command not found` / `'sbt' is not recognized`

The installer updated your `PATH`, but your terminal still has the old one.
**Close and reopen the terminal.** On Windows you may need to log out and back
in.

### The wrong Java version is used

Check what is actually on your `PATH`:

```bash
java -version
```

If it reports 25, 26, or 8, you have another JDK taking precedence. Either
uninstall it, or set `JAVA_HOME` to your JDK 21 and put `$JAVA_HOME/bin` first
on your `PATH`. With SDKMAN you can switch per shell:

```bash
sdk use java 21.0.9-tem
```

### Compilation errors that mention the JDK or `class file version`

Almost always a too-new JDK. Confirm `java -version` reports **21** or **17**;
see the warning in [Requirements](#requirements).

### VS Code shows no completions or every symbol is red

In order:

1. Did you open the folder that directly contains `build.sbt`?
2. Did the **Import build** step finish?
3. Run **Metals: Run Doctor** and read what it reports.
4. Run `Metals: Restart build server`, then re-import.

### Nothing works and you want a clean slate

Delete the generated build directories and re-import:

```bash
rm -rf .bsp .bloop .metals target project/target
```

Then reopen the folder in VS Code and run **Metals: Import build** again.

### Still stuck

Ask on the course LMS **Board > Q&A** with:

* your OS and version,
* the output of `java -version` and `sbt --version`,
* the exact error message.
