# luau.parallel
Implementation of the match/switch system in Luau

# usage
In order to use, you must call the function required with the value, which returns another function that you must call with your map of possibilities. In a nutshell, if the index of the map is a function, the runner will call that function with the value, and if that function returns true, then it's corresponding value will be returned; if the index is a non-function (excluding `none` and `other`), it will simply check if that key equals the value-to-match; if the index is `none`, it will be computed if the value-to-match is nil, and if it's `other`, then it will be computed if the value-to-match has no other possibilities on the map.

By the way, if the key-value is a function, it will call that function with the value-to-match and the parallel call will return whatever value that function returns. Otherwise, it will just return whatever the key-value is. If you genuinely intend on using a function as a value, simply wrap the function in curlies; e.g:

```lua
local parallel = require(...)

function addFive(a)
  return a + 5
end

local result = parallel (10) {
  [10] = addFive, -- In this case, the function will be called with the value-to-match, and the value returned by the parallel statement will be whatever the function given returns!
  [12] = {addFive} -- In this case, the function will not be ran, but instead, returned. Don't worry about the curlies; they'll be removed during computation.
} 
```

Here's a general example:

```lua
local parallel = require(...)

function is_even(v)
	return v % 2 == 0
end
function is_odd(v)
	return (not is_even(v)) 
end

local result = parallel (10) {
	[is_even] = 2,
	[is_odd] = 1,
	other = 0
}

warn(result) -- '2'

```
