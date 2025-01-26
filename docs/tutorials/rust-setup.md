# Rust Tutorial: Setting up a Dev Container for Rust
* Primary author: [Ben Edwards](https://github.com/bkedwards)
* Reviewer: [Abiye Berhanu](https://github.com/aberhanu)

## Prerequisites
1. `GitHub`: Make sure you have set up a [GitHub account](https://github.com/).
2. `Git`: Install [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
3. `Visual Studio Code`: Download VS Code [here](https://code.visualstudio.com/).
4. `Docker`: Install [Docker](https://www.docker.com/products/docker-desktop/) so you can run the dev container.

## Part 1. Project Setup: Creating the Repository

### Step 1. Create a Local Directory and Initialize Git

(A) Open your terminal or command prompt

(B) Create a new directory for your project.
```sh
mkdir rust-tutorial
cd rust-tutorial
``` 
!!! note 
    Of course, if you'd like to organize this tutorial somewhere else on your machine, go ahead and change into that parent directory first. By default this will be in your user's home directory.

(C) Initialize a new Git repository
```sh
git init
```

(D) Create a README file:
```sh
echo "# COMP 423 Rust Tutorial" > README.md
git add README.md
git commit -m "Initial commit with README"
```

### Step 2. Create a Remote Repository on GitHub
(1) Log in to your GitHub account and navigate to the [Create a New Repository page](https://github.com/new)

(2) Fill in the details as follows:

- **Repository Name:** `rust-tutorial`

- **Description:** "Tutorial for a simple Hello World program in Rust using Dev Container."

- **Visibility:** Public

(3) Do not initalize the repository with a README. .gitignore, or license

(4) Click **Create Respository**

### Step 3. Link your Local Repository to GitHub
(1) Add the Github repository as a remote
```sh
git remote add origin https://github.com/<your-username>/rust-tutorial.git
```
Replace `<your-username` with your GitHub username

(2) Check your default branch name with the subcommand `git branch`. If it's not `main`, rename it to `main` with the following command: `git branch -M main`. 
!!!note
    Old versions of `git` choose the name `master` for the primary branch, but these days `main` is the standard primary branch name.

(3) Push your local commits to the GitHub repository
```sh
git push --set-upstream origin main
```
!!! note "Understanding the --set-upstream Flag"

    The `git push --set-upstream origin main` command pushes the main branch to the remote repository named origin.

    - The `--set-upstream` flag establishes a connection between your local main branch and the remote branch, allowing future pushes and pulls to be done simply with git push or git pull without needing to specify the branch name.

        - `-u` is an abreviation for the `--set-upstream` flag

(4) Back in your web browser, refresh your GitHub repository to see that the same commit you made locally has now been pushed to remote. 

- You can use `git log` locally to see the commit ID and message which should match the ID of the most recent commit on GitHub. This is the result of pushing your changes to your remote repository.

## Part 2. Setting up the Development Environment
### Step 1. Add Development Container Configuration
1. In VS Code open the `rust-tutorial` directory. You can do this via File > Open Folder

2. Install the **Dev Containers** extension for VS Code
3. Create a `.devcontainer` directory in the root of your project with the following file inside of this "hidden" configuration directory:

**```
.devcontainer/devcontainer.json
```**

The `devcontainer.json` file defines the configuration for your development environment. Here, we're specifying the following:

* **name:** A descriptive name for your dev container.

* **image:** The Docker image to use, in this case, the latest version of a Rust environment. [Microsoft maintains a collection of base images for many programming language environments](https://hub.docker.com/r/microsoft/vscode-devcontainers), but you can also create your own!

* **customizations:** Adds useful configurations to VS Code, like installing the Rust extension. When you search for VSCode extensions on the marketplace, you will find the string identifier of each extension in its sidebar. Adding extensions here ensures other developers on your project have them installed in their dev containers automatically.

* **postCreateCommand:** Commands to be executed after the container is created. In our case, there is nothing to be run after creation.

```json
{
"name": "Rust Tutorial",
"image": "mcr.microsoft.com/devcontainers/rust:latest",
"customizations": {
	"vscode:": {
		"settings":{},
		"extensions":["rust-lang.rust-analyzer"]
	}
},
"postCreateCommand": ""	
}
```

### Step 5. Reopen the Project in a VSCode Dev Container

Reopen the project in the container by pressing `Ctrl+Shift+P`  (or `Cmd+Shift+P` on Mac), typing "Dev Containers: Reopen in Container," and selecting the option. 

- This may take a few minutes while the image is downloaded and the requirements are installed.

Once your dev container setup completes, close the current terminal tab (trash can), open a new terminal pane within VSCode, and try running `rustc --version` to that your dev container is running an updated version of Rust.

!!! note "`rustc --version`"
	As of January 2025 `rustc 1.83.0` is the most up-to-date version of Rust.

## Part 3. Creating a Rust Binary File and Building it 

[Cargo](https://doc.rust-lang.org/book/ch01-03-hello-cargo.html) is the build system and package manager for the Rust language, and is employed by most developers to manage their Rust projects.

(1) To start a new package with Cargo, use the `cargo new` command to create a binary project:
```sh
cargo new hello_cargo_COMP423 --vcs none
cd hello_cargo_COMP423
```

!!! note "`--vcs none`"
    The `--vcs none` flag tells Cargo not to create a new git repository for your project. Be sure to use this if you want to manage version control yourself or if there is already a repository existing for your project.

Go into the `hello_cargo_COMP423` directory and list all the files (`ls`). You’ll see that Cargo has generated two files and one directory for us: a `Cargo.toml` file and a `src` directory with a `main.rs` file inside.

(3) Open `src/main.rs` and take a look:
```rs
fn main() { 
	println!("Hello, world!"); 
}
```
Change the text to the following:
```rs
fn main() { 
	println!("Hello COMP423!"); 
}
```
(4) Go back to the terminal, and compile the [binary crate](https://doc.rust-lang.org/book/ch07-01-packages-and-crates.html) using
```sh
$ cargo build
	Compiling hello_cargo_COMP423 v0.1.0 (workspace/rust-tutorial/hello_cargo_COMP423)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 3.90s
```
And then run it using
```sh
$ ./target/debug/hello_cargo_COMP423
# or .\target\debug\hello_cargo.exe on Windows Hello, world!
Hello COMP423!
```
(5) Alternatively, you can also use `cargo run` to compile the code and run the resultant executable all in one command:
```sh
$ cargo run 
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.10s
    Running `target/debug/hello_cargo_COMP423`
Hello COMP423!
```
!!! note "Cargo Run"
    `cargo run` is similar to `cargo build` but is more convenient as you don't have to run `cargo build` and then execute the whole path of the binary.

    - Most Rust developers prefer to use `cargo run`.

##Conclusion
**Congratulations!**

You’ve successfully created a simple Hello World program in the Rust language and configured a development environment. These are important foundational skills that can be applied to many open-source and professional projects, and can help kick-start your illustrious Rust career! 