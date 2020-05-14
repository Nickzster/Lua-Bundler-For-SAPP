# Lua File Bundler

## Motivation

Halo Custom Edition's SAPP allows additional functionality using the lua language. However, you must load these files through a configuration file. This makes it extremely difficult to modularize larger SAPP projects. To circumvent this, the Lua-Bundler-For-SAPP adds additional functionality to lua files.

## How to use the bundler

## Example

While this example is contrived, it clearly demonstrates how the bundler works:

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
