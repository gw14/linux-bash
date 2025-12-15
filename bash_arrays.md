Here’s a **compact Bash arrays cheat sheet** you can keep handy:

---

# **Bash Arrays Cheat Sheet**

## 1. **Declare Arrays**

```bash
# Indexed array
my_array=(apple banana cherry)

# Add elements individually
my_array[0]=apple
my_array[1]=banana

# Associative array (Bash 4+)
declare -A colors
colors[apple]=red
colors[banana]=yellow
```

---

## 2. **Access Elements**

```bash
echo "${my_array[0]}"    # first element
echo "${my_array[@]}"    # all elements
echo "${my_array[*]}"    # all elements as single word
echo "${#my_array[@]}"   # number of elements
echo "${#my_array[1]}"   # length of element at index 1

# Associative arrays
echo "${colors[apple]}"  # red
```

---

## 3. **Add / Remove Elements**

```bash
# Append
my_array+=(date)

# Remove
unset my_array[1]  # removes element at index 1
```

> ⚠️ `unset` leaves a gap in indexes.

---

## 4. **Iterate Over Arrays**

```bash
# By index
for i in "${!my_array[@]}"; do
    echo "$i: ${my_array[$i]}"
done

# By value
for val in "${my_array[@]}"; do
    echo "$val"
done

# Associative array
for key in "${!colors[@]}"; do
    echo "$key -> ${colors[$key]}"
done
```

---

## 5. **Slices / Subarrays**

```bash
echo "${my_array[@]:1:2}"  # elements from index 1, length 2
echo "${my_array[@]:1}"    # elements from index 1 to end
```

---

## 6. **Check if Element Exists**

```bash
item="banana"
if [[ " ${my_array[@]} " =~ " $item " ]]; then
    echo "$item exists"
fi
```

---

## 7. **Useful Notes**

* Arrays are **0-indexed**.
* `"$array[@]"` preserves spaces; `$array[@]` splits words.
* No native multidimensional arrays (simulate with associative arrays or delimiters).
* Associative arrays require `declare -A`.
* Bash 3: only indexed arrays.

---

Bash arrays are a very handy feature for managing collections of data in shell scripts. Here's a thorough overview:

---

## 1. **Basic Array Syntax**

### Declaring an Array

```bash
# Method 1: explicit
my_array=(apple banana cherry)

# Method 2: assigning elements individually
my_array[0]=apple
my_array[1]=banana
my_array[2]=cherry
```

### Accessing Elements

```bash
echo "${my_array[0]}"  # apple
echo "${my_array[2]}"  # cherry
```

### All Elements

```bash
echo "${my_array[@]}"   # apple banana cherry
echo "${my_array[*]}"   # apple banana cherry (treated slightly differently in quoting)
```

### Length of Array

```bash
echo "${#my_array[@]}"  # 3 elements
```

### Length of Element

```bash
echo "${#my_array[1]}"  # length of "banana" -> 6
```

---

## 2. **Adding and Removing Elements**

```bash
# Add an element
my_array+=(date)

# Remove element
unset my_array[1]  # removes "banana"
```

> Note: `unset` removes the element but does not reindex the array. Index 1 will be empty.

---

## 3. **Iterating Over Arrays**

```bash
# Using index
for i in "${!my_array[@]}"; do
    echo "$i: ${my_array[$i]}"
done

# Using values directly
for val in "${my_array[@]}"; do
    echo "$val"
done
```

---

## 4. **Slices and Subarrays**

```bash
# Subarray starting at index 1, length 2
echo "${my_array[@]:1:2}"  # banana cherry

# From index 1 to the end
echo "${my_array[@]:1}"
```

---

## 5. **Associative Arrays (Bash 4+)**

Associative arrays let you use strings as keys:

```bash
declare -A colors
colors[apple]="red"
colors[banana]="yellow"

echo "${colors[apple]}"  # red
```

Iterating over keys:

```bash
for key in "${!colors[@]}"; do
    echo "$key -> ${colors[$key]}"
done
```

---

## 6. **Important Notes / Gotchas**

* Quoting matters: `"$array[@]"` preserves elements with spaces, while `$array[@]` splits on spaces.
* Arrays are zero-indexed.
* Bash does **not** support multidimensional arrays natively. You can simulate them using associative arrays or by using delimited strings.
* Array operations (append, unset) do **not** automatically reindex the array; gaps can exist.
* Associative arrays require `declare -A` and only exist in Bash 4+.

---

## 7. **Practical Examples**

### Example 1: Loop and Modify

```bash
fruits=(apple banana cherry)
for i in "${!fruits[@]}"; do
    fruits[$i]="$i-${fruits[$i]}"
done
echo "${fruits[@]}"  # 0-apple 1-banana 2-cherry
```

### Example 2: Check if Element Exists

```bash
item="banana"
if [[ " ${fruits[@]} " =~ " $item " ]]; then
    echo "$item exists in the array"
fi
```

---


