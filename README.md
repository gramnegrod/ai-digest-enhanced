# ai-digest

I took khromov Stanislav Khromov's idea and made a small tweak.  This is the method how I learned to role this out:  Basically you have to make an ignore file, it should catch the big stuff although if it is too big you will haved to find and include those files on your won, the make a package,json file if not already there (add to it if it is there), then run the npm command to make a codebase file to use to give tho your llm to increase its knowledge of your base.  Redo the codebase file every time you make significant changes.:Using AI-Digest for Codebase Documentation
This project uses ai-digest to generate a Markdown file summarizing the codebase. Follow the instructions below to set up and use ai-digest effectively in this project.

This assumes npm is installed!

Prerequisites
Ensure that ai-digest is cloned in a known directory on your system. For this guide, we'll use C:\VS Code\ai-digest.

CLONE IF NOT DONE BEFORE:
git clone https://github.com/khromov/ai-digest.git "C:\VS Code\ai-digest"




Setting Up Your Project
Create .aidigestignore Generator Script
Create a file named generate-aidigestignore.js in your project directory with the following content:

const fs = require('fs');
const ignorePatterns = `
# Node modules and environment files
node_modules
.env

# Distribution and build directories
dist
build
out
lib

# Python virtual environments
venv
*.pyc
__pycache__

# Logs and temporary files
*.log
tmp
temp
*.tmp

# Configuration and system files
.DS_Store
Thumbs.db
.idea
.vscode
*.iml

# Git and version control
.git
.gitignore

# Test directories
test
tests
coverage

# Binary and compiled files
*.exe
*.dll
*.so
*.dylib
*.bin
*.dat
*.bak

# Documentation
*.md
docs

# Miscellaneous
*.swp
*.swo
*.swn
*.bak
*.old
*.orig
*.rej
*.sublime-project
*.sublime-workspace

# Custom exclusions
alloy-voice-assistant/venv
alloy-voice-assistant/data
alloy-voice-assistant/config
alloy-voice-assistant/Sonnet35UI/test.py
alloy-voice-assistant/Sonnet35UI/systemprompt.txt
alloy-voice-assistant/Sonnet35UI/saved_conversations.json
`;

fs.writeFile('.aidigestignore', ignorePatterns.trim(), (err) => {
  if (err) {
    console.error('Error writing .aidigestignore file:', err);
  } else {
    console.log('.aidigestignore file generated successfully.');
  }
});






**Update package.json**

Add the following scripts to your package.json OR MAKE A NEW ONE:


{
  "name": "chatbot",
  "version": "1.0.0",
  "description": "Description of your project",
  "main": "index.js",
  "scripts": {
    "generate-ignore": "node generate-aidigestignore.js",
    "generate-docs": "npm run generate-ignore && npx ts-node ../ai-digest/src/index.ts"
  }
}





**Generate .aidigestignore File**

Run the following command to generate the .aidigestignore file:


npm run generate-ignore




**Generate codebase.md File**

Run the following command to generate the codebase.md file:


npm run generate-docs



**Customizing .aidigestignore**

You can modify the generate-aidigestignore.js script to add more patterns as needed. This script helps ensure that your .aidigestignore file is always up-to-date with the latest exclusions.

**Summary**
Ensure ai-digest is in the correct directory (C:\VS Code\ai-digest).
Use npm run generate-ignore to create the ignore file.
Use npm run generate-docs to generate the documentation file.
By following these steps, you can easily use ai-digest in any project with minimal hassle.


***Original Readme***

A CLI tool to aggregate your codebase into a single Markdown file for use with Claude Projects or custom ChatGPTs.

## Features

- Aggregates all files in the specified directory and subdirectories
- Ignores common build artifacts and configuration files
- Outputs a single Markdown file containing the whole codebase
- Provides options for whitespace removal and custom ignore patterns

## How to Use

Start by running the CLI tool in your project directory:

```bash
npx ai-digest
```

This will generate a `codebase.md` file with your codebase.

Once you've generated the Markdown file containing your codebase, you can use it with AI models like ChatGPT and Claude for code analysis and assistance.

### With ChatGPT:
1. Create a Custom GPT
2. Upload the generated Markdown file to the GPT's knowledge base

### With Claude:
1. Create a new Project
2. Add the Markdown file to the Project's knowledge

For best results, re-upload the Markdown file before starting a new chat session to ensure the AI has the most up-to-date version of your codebase.

## Options

- `-i, --input <directory>`: Specify input directory (default: current directory)
- `-o, --output <file>`: Specify output file (default: codebase.md)
- `--no-default-ignores`: Disable default ignore patterns
- `--whitespace-removal`: Enable whitespace removal
- `--show-output-files`: Display a list of files included in the output
- `--help`: Show help

## Examples

1. Basic usage:

   ```bash
   npx ai-digest
   ```

2. Specify input and output:

   ```bash
   npx ai-digest -i /path/to/your/project -o project_summary.md
   ```

3. Enable whitespace removal:

   ```bash
   npx ai-digest --whitespace-removal
   ```

4. Show list of included files:

   ```bash
   npx ai-digest --show-output-files
   ```

5. Combine multiple options:

   ```bash
   npx ai-digest -i /path/to/your/project -o project_summary.md --whitespace-removal --show-output-files
   ```

## Custom Ignore Patterns

ai-digest supports custom ignore patterns using a `.aidigestignore` file in the root directory of your project. This file works similarly to `.gitignore`, allowing you to specify files and directories that should be excluded from the aggregation.

Use the `--show-output-files` flag to see which files are being included, making it easier to identify candidates for exclusion.


## Whitespace Removal

When using the `--whitespace-removal` flag, ai-digest removes excess whitespace from files to reduce the token count when used with AI models. This feature is disabled for whitespace-dependent languages like Python and YAML.

## Binary and SVG File Handling

Binary files and SVGs are included in the output with a note about their file type. This allows AI models to be aware of these files without including their full content.

## Local Development

Run `npm run start` to run the CLI tool on the local project. (Very meta!)

Run `npm test` to run the tests.

## Deploy New Version

```
npm publish
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License.
