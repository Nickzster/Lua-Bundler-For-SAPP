# Lua File Bundler

## Motivation

Halo Custom Edition's SAPP allows additional functionality using the lua language. However, you must load these files through a configuration file. This makes it extremely difficult to modularize larger SAPP projects. To circumvent this, the Lua-Bundler-For-SAPP adds additional functionality to lua files.

## How to use the bundler

### Import Statements

- Use import statements to declare that a certain file is using an object or a value that is defined in a different file.
- Import statements are done relatively to the project directory, NOT relative to the current file.
- To import properly, you need to follow the correct syntax: `-- import project-dir.directory.to.file.filename`
- import statements need to be wrapped between a `-- BEGIN_IMPORT` and an `-- END_IMPORT` statement

### Ignore Statements

- Everything wrapped between a `-- BEGIN_IGNORE` and an `-- END_IGNORE` will be ignored in the final output file.

### Running the bundler

- Before running the bundler, you need to configure some options. You have two choices:
  1. Open the `bundler.py` file and modify the configuration files to your liking. Make sure the bundler.py file lives directly outside of the project you want to build.
  2. Run bundler.py with arguments. The proper way to run it is as follows: `python bundler.py sapp-api-version-number project-name project-dir-name entry-file`
  - `sapp-api-version` is the api version that goes on top of every SAPP file
  - `project-name` is the name of the output file.
  - `project-dir-name` is the location of the root folder
  - `entry-file` is the name of the entry file (I.E, main.lua)

## Example

While this example is contrived, it clearly demonstrates how the bundler works:

### File Structure

```
* bundler.py
- example-one/
  - components/
    * N1.lua
    * N3.lua
  - util/
    * N2.lua
  * main.lua
  * N4.lua
```

### Files

`example-one/main.lua`

```lua

-- BEGIN_IMPORT
-- import example-one.components.N1 end
-- import example-one.util.N2 end
-- END_IMPORT

--Will appear 5th.
function main()
    print("In main!")
end

-- BEGIN_IGNORE
function test()
    print("Test!")
end
-- END_IGNORE
```

`example-one/components/N1.lua`

```lua
-- BEGIN_IMPORT
-- import example-one.util.N2 end
-- import example-one.components.N3 end
-- import example-one.N4 end
-- END_IMPORT

--Will appear 4th.
function N1()
    print("In N1")
end

-- BEGIN_IGNORE
function test()
    print("Test!")
end
-- END_IGNORE
```

`example-one/util/N2.lua`

```lua
-- BEGIN_IMPORT
-- import example-one.N4 end
-- END_IMPORT

-- Will appear 3rd
function N2()
    print("In N2")
end

-- BEGIN_IGNORE
function test()
    print("Test!")
end
-- END_IGNORE
```

`example-one/components/N3.lua`

```lua
-- NO_IMPORTS

--Will appear first
function N3()
    print("In N3!")
end

-- BEGIN_IGNORE
function test()
    print("Test!")
end
-- END_IGNORE
```

`example-one/N4.lua`

```lua
-- BEGIN_IMPORT
-- import example-one.components.N3 end
-- END_IMPORT

--Will appear second
function N4()
    print("In N4!")
end

-- BEGIN_IGNORE
function test()
    print("Test!")
end
-- END_IGNORE
```

`OUTPUT example-one.lua`

```lua
api_version="1.10.1.0"


--Will appear first
function N3()
    print("In N3!")
end


--Will appear second
function N4()
    print("In N4!")
end


-- Will appear 3rd
function N2()
    print("In N2")
end


--Will appear 4th.
function N1()
    print("In N1")
end


--Will appear 5th.
function main()
    print("In main!")
end

```
