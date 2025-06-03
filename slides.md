---
theme: default
background: 'linear-gradient(45deg, #1e3c72, #2a5298)'
style: ./style.css
class: text-center
highlighter: shiki
lineNumbers: true
info: |
  ## C++ Performance Optimization Workshop
  Learn to write blazingly fast C++ code! 🚀
drawings:
  persist: false
transition: slide-left
title: C++ Performance Optimization Mastery
---
---
# C++ Performance Optimization Mastery 🚀

## Blazingly Fast Code & Furious Optimizations 🔥💻
<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for Next <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: center
class: text-center
---

# Agenda/Roadmap 🗺️

- **Why Performance Matters** ⚡ (5 min) - The Need for Speed!
- **Fundamentals** 🧠 (10 min) - Mindset & Tools
- **Core Techniques** 🏎️ (20 min) - Algorithms, Data Structures, Memory
- **Advanced Techniques** ✨ (25 min) - Loops, Branch Prediction, SIMD, TMP
- **Modern C++ Features** 🛠️ (15 min) - Move Semantics, STL, Compiler Magic
- **Benchmarking** 📊 (10 min) - Measure, Don't Guess!
- **Case Studies** 🎯 (15 min) - Real-World Examples
- **Pitfalls & Anti-Patterns** ⚠️ (10 min) - What Not To Do
- **Future & Advanced** 🔮 (10 min) - C++20/23, Parallelism
- **Q&A** ❓ (10 min)

**Learning Objectives:** Understand, identify, and apply C++ performance optimization techniques.

---
layout: default
---

# Why Performance Matters ⚡

- **User Experience**: Slow applications frustrate users. Milliseconds matter! 😠➡️😃
- **Resource Costs**: Efficient code requires less hardware, saving 💰 on servers and power.
- **Scalability**: Performant code handles more users/data without degradation. 📈
- **Energy Efficiency**: Faster code consumes less power, better for the 🌍.

## The Cost of Slow Code
- Abandoned shopping carts 🛒
- Lower search engine rankings 📉
- Reduced user engagement 💔
- Competitive disadvantage 🐌

:::กาซ้อน
layout: center
---
<img src="/memes/meme_O_n_factorial_horror.txt" alt="Meme: O(n!) horror - A character looking horrified/panicked at a computer screen" class="mx-auto my-4 max-h-60">
:::
---
layout: default
---

# Performance Mindset 🧠

**"Measure twice, cut once" applies to code too!**

1.  **Measure, Don't Guess**: Your intuition about bottlenecks can be wrong. Profiling data is key! 🎯
2.  **Premature Optimization is the Root of (Some) Evil**:
    > "We should forget about small efficiencies, say about 97% of the time: premature optimization is the root of all evil. Yet we should not pass up our opportunities in that critical 3%." - Donald Knuth
3.  **Balance Performance & Readability**: Optimized code can be complex. Strive for clarity first, then optimize where needed. Readable code is maintainable code. ✨
4.  **Understand the "Why"**: Why is this code slow? What are the constraints? (CPU, memory, I/O)
5.  **Set Goals**: What's "fast enough"? Define performance targets. 🏁

---
layout: default
---

# Profiling Tools 📊

**Shining a light on your code's hotspots!** 🔥

Profilers help you understand where your program spends its time and resources.

**Common C++ Profilers:**
-   **Valgrind (Callgrind/Cachegrind)**: Linux, detailed call graphs, cache simulation.
    -   `valgrind --tool=callgrind ./your_program`
-   **gprof**: Part of GCC, relatively simple to use.
    -   `g++ -pg your_code.cpp -o your_program`
    -   `./your_program`
    -   `gprof your_program gmon.out > analysis.txt`
-   **Perf**: Linux kernel-based, powerful, low-overhead.
    -   `perf record ./your_program`
    -   `perf report`
-   **Visual Studio Profiler**: Windows, integrated into VS IDE.
-   **Xcode Instruments**: macOS, integrated into Xcode.
-   **Intel VTune Profiler**: Advanced, cross-platform.

**Example Workflow (Conceptual):**
1. Compile with debug symbols (`-g`).
2. Run program under profiler.
3. Analyze profiler output (e.g., flame graphs, call trees).
4. Identify bottlenecks.
5. Optimize.
6. Repeat.

<!-- TODO: Add a visual chart/graph placeholder if possible, or describe what it would show -->
<!-- Visual: A pie chart showing time distribution in a sample program -->

---
layout: default
---

# Big O Notation Refresher 📈

**Understanding how your algorithm scales as input grows.**

| Notation | Name          | Example                     | Performance |
| :------- | :------------ | :-------------------------- | :---------- |
| O(1)     | Constant      | Array access `arr[i]`       | Excellent 🔥|
| O(log n) | Logarithmic   | Binary search               | Great 👍    |
| O(n)     | Linear        | Loop through a list         | Good ✅    |
| O(n log n)| Linearithmic  | Efficient sorts (quicksort) | Fair 👌    |
| O(n²)    | Quadratic     | Nested loops (naive sort)   | Poor 😟    |
| O(2ⁿ)    | Exponential   | Recursive Fibonacci         | Bad 😱     |
| O(n!)    | Factorial     | Travelling Salesman (naive) | Awful 💀   |

<!-- TODO: Add a visual complexity chart placeholder -->
<!-- Visual: A graph showing different Big O curves (n vs time) -->

**Key takeaway**: Algorithmic complexity often has the BIGGEST impact on performance for large inputs.

:::กาซ้อน
layout: center
---
<img src="/memes/meme_O_n_squared_oops.txt" alt="Meme: O(n^2) oops - A character sweating profusely" class="mx-auto my-4 max-h-60">
:::
---
layout: default
---

# 🏎️ Core Optimization Techniques

## Algorithm Optimization 🎯

**Choosing the right algorithm is often the single most effective optimization.**

-   A better algorithm can yield orders of magnitude improvement, especially for large `n`.
-   Example: Sorting a large dataset.

```cpp
// Bad: Bubble Sort - O(n²)
void bubbleSort(std::vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                std::swap(arr[j], arr[j + 1]);
            }
        }
    }
}

// Better: std::sort (IntroSort) - O(n log n)
#include <algorithm>
#include <vector>

void efficientSort(std::vector<int>& arr) {
    std::sort(arr.begin(), arr.end());
}
```
-   **Benchmark**: For 100,000 elements, `std::sort` can be thousands of times faster! 🚀

---
layout: default
---

# Data Structure Selection 📦

**The right container for the job can make a huge difference.**

Consider:
-   **Access patterns**: Sequential access? Random access? Frequent insertions/deletions at ends or middle?
-   **Memory overhead**: How much extra memory does the structure use?
-   **Cache locality**: Are elements stored contiguously?

| Data Structure        | Use Case                                  | Pros                               | Cons                                     |
| :-------------------- | :---------------------------------------- | :--------------------------------- | :--------------------------------------- |
| `std::vector`         | Dynamic array, general purpose            | Fast random access, cache-friendly | Slow middle insert/delete                |
| `std::list`           | Doubly-linked list                        | Fast insert/delete anywhere        | Slow random access, poor cache locality  |
| `std::deque`          | Double-ended queue                        | Fast front/back insert/delete      | Slower than vector, complex memory       |
| `std::map`            | Sorted associative array (Red-Black Tree) | Ordered keys, `log n` access       | Higher overhead than `unordered_map`     |
| `std::unordered_map`  | Hash table                                | Average `O(1)` access              | Unordered, worst-case `O(n)`             |

<!-- TODO: Add visual diagrams of data structures -->
<!-- Visual: Simple diagrams of vector (contiguous blocks), list (nodes and pointers), hash table (buckets) -->

**Memory Layout Importance**: Contiguous memory (like in `std::vector`) is generally better for CPU caches.

---
layout: default
---

# Memory Management Magic 🪄

**Efficiently using memory can significantly boost performance.**

-   **Stack vs. Heap Allocation**:
    -   **Stack**: Fast allocation/deallocation (pointer bump). Limited size. Automatic cleanup.
    -   **Heap**: Slower (`new`/`delete`). Flexible size. Manual or smart pointer cleanup.
    -   Prefer stack allocation for small, short-lived objects.

-   **RAII (Resource Acquisition Is Initialization)**: Use objects to manage resources (memory, files, locks). Destructors automatically release resources. `std::unique_ptr`, `std::shared_ptr`, `std::vector`.

-   **Smart Pointers Performance**:
    -   `std::unique_ptr`: Zero-cost abstraction over raw pointers (usually).
    -   `std::shared_ptr`: Adds overhead for reference counting. Use when shared ownership is necessary.

-   **Memory Pool Techniques**: Pre-allocate a large chunk of memory and manage it yourself. Reduces `new`/`delete` overhead for frequent allocations of same-sized objects. (Advanced)

```cpp
// Potentially Slow: Multiple small heap allocations
std::vector<std::unique_ptr<MyObject>> objects;
for(int i = 0; i < 1000; ++i) {
    objects.push_back(std::make_unique<MyObject>()); // new called 1000 times
}

// Better: Reserve memory once for vector's internal buffer
std::vector<MyObject> objects_val; // Objects stored by value
objects_val.reserve(1000); // Allocate memory for 1000 objects upfront
for(int i = 0; i < 1000; ++i) {
    objects_val.emplace_back(); // Construct in-place, potentially avoids copies
}
```

---
layout: default
---

# Cache-Friendly Programming 🗄️

**Helping the CPU access data faster.**

-   **CPU Cache Hierarchy**: L1, L2, L3 caches are small, fast memory on the CPU. Main memory (RAM) is much slower.
    -   L1: Fastest, smallest (e.g., 32-64KB)
    -   L2: Faster, medium (e.g., 256KB-1MB)
    -   L3: Fast, larger (e.g., several MBs)
    -   RAM: Slowest, largest (e.g., GBs)

-   **Cache Miss Cost**: When data isn't in a cache, CPU stalls waiting for it from RAM. This is expensive! ⏳

-   **Data Locality Principles**:
    1.  **Temporal Locality**: Data accessed recently is likely to be accessed again soon.
    2.  **Spatial Locality**: Data near recently accessed data is likely to be accessed soon. (Caches load data in "cache lines" - typically 64 bytes).

-   **How to be cache-friendly**:
    -   Use contiguous memory structures (`std::vector`, arrays).
    -   Iterate over data sequentially.
    -   Organize data to be accessed together (e.g., struct of arrays vs. array of structs, depending on access patterns).
    -   Avoid pointer chasing through memory.

<!-- TODO: Add structure padding visualization -->
<!-- Visual: Diagram showing how struct members might be padded to align with cache lines, or how reordering struct members can improve packing. -->

---
layout: center
class: text-center
---

# Cache Misses Be Like... 😭

<img src="/memes/meme_cache_miss_pain.txt" alt="Meme: Cache misses be like - A character waiting impatiently" class="mx-auto my-4 max-h-60">

*(Imagine the CPU sighing dramatically)*
---
layout: default
---

# ⚡ Advanced Optimization Techniques

## Loop Optimizations 🔄

**Loops are often hotspots. Small changes can yield big gains.**

-   **Loop Unrolling**: Reduce loop overhead by performing more work per iteration.
    -   Benefit: Fewer branch instructions, more instruction-level parallelism.
    -   Caution: Can increase code size, potentially hurting cache. Compilers often do this automatically (`-O2`/`-O3`).
-   **Loop Fusion (Merging)**: Combine adjacent loops that iterate over the same range.
    -   Benefit: Improves data locality, reduces loop overhead.
-   **Avoiding Function Calls in Loops**: Function call overhead can be significant in tight loops.
    -   Consider inlining (`inline` keyword, or compiler decision) or moving calculations out.
-   **Strength Reduction**: Replace expensive operations with cheaper ones (e.g., `x * 2` with `x << 1`). Compilers are good at this.
-   **Cache-Efficient Iteration**: Iterate in a way that respects cache lines (e.g., row-major vs. column-major access in 2D arrays).

```cpp
// Unoptimized: Function call in loop
for(int i = 0; i < size; ++i) {
    total_sum += data[i] * expensive_calculation(data[i]); // Call per iteration
}

// Potentially Optimized: Hoist invariant calculations or simplify
// If expensive_calculation(data[i]) always yields same factor for data[i]
// or can be simplified.
// Example: if it's just multiplying by a constant factor
const int factor = get_factor(); // Hoisted
for(int i = 0; i < size; ++i) {
    total_sum += data[i] * factor;
}

// Optimized (if expensive_calculation is complex but its result can be precomputed for possible inputs):
// auto precomputed_results = precompute_for_all_possible_values(data_range);
// for(int i = 0; i < size; ++i) {
//    total_sum += data[i] * precomputed_results[data[i]];
// }
```

---
layout: default
---

# Branch Prediction 🎯

**Helping the CPU guess the future of your `if` statements.**

-   **How it Works**: Modern CPUs try to predict the outcome of branches (e.g., `if`, `switch`, loops) to keep the instruction pipeline full.
-   **Misprediction Penalty**: If the prediction is wrong, the pipeline must be flushed and refilled, costing cycles (can be 10-20+ cycles!).
-   **Be Predictable**: Write code where branch outcomes are consistent.
    -   Sort data before processing if it makes branches more predictable.
    -   Avoid data-dependent branches in critical loops if possible.
-   **Likely/Unlikely Hints (C++20 `[[likely]]`, `[[unlikely]]`)**:
    ```cpp
    if (very_rare_condition) [[unlikely]] {
        // ... error handling ...
    } else [[likely]] {
        // ... common path ...
    }
    ```
    These attributes can help the compiler optimize code layout for better branch prediction.
-   **Branchless Programming**: Replace branches with arithmetic or bitwise operations.
    -   Can be faster if mispredictions are common, but code can be less readable.
    -   Example: `int max_val = (a > b) ? a : b;` vs. `int max_val = b + ((a - b) & -(a > b));` (use with caution and benchmark!)

---
layout: default
---

# SIMD & Vectorization 🚀

**Single Instruction, Multiple Data - Processing multiple data points with one instruction.**

-   **Concept**: Modern CPUs have special registers and instructions that can perform operations (e.g., addition, multiplication) on multiple data elements simultaneously (e.g., 4 floats, 8 shorts).
-   **Auto-Vectorization**: Compilers (`-O3`, `-Ofast`) try to automatically convert scalar loops into vectorized code.
    -   **Tips for Auto-Vectorization**:
        -   Simple loops with no data dependencies between iterations.
        -   Use arrays/`std::vector` with contiguous memory.
        -   Avoid complex control flow inside loops.
        -   Align your data (e.g., `alignas(16)`).
        -   Check compiler reports (`-fopt-info-vec-all` for GCC/Clang).
-   **Manual SIMD with Intrinsics**: For maximum control, use compiler-specific intrinsic functions (e.g., `_mm_add_ps` for SSE).
    -   Platform-specific and can be complex, but offers highest potential gains.
    -   Libraries like Intel's IPP or Eigen can simplify this.
-   **Performance Gains**: Can be significant (2x, 4x, or more) for suitable workloads like image processing, physics simulations, scientific computing.

<!-- Visual: Diagram showing scalar (one-by-one) vs. SIMD (multiple at once) processing -->

---
layout: default
---

# Template Metaprogramming (TMP) 🎭

**Performing computations at compile-time instead of runtime.**

-   **Concept**: Use C++ templates to generate code or compute values during compilation.
-   **Benefits**:
    -   **Zero-Cost Abstractions**: Create abstractions that have no runtime overhead.
    -   **Performance**: Shift work from runtime to compile time.
    -   **Type Safety**: Perform checks and generate type-safe code.
-   **`constexpr` Functions (C++11 and later)**: The modern way to do compile-time computations.
    -   Functions evaluated at compile time if arguments are compile-time constants.
    -   Much more readable than traditional TMP.

```cpp
// Runtime factorial
long long factorial_runtime(int n) {
    return (n <= 1) ? 1 : n * factorial_runtime(n - 1);
}

// Compile-time factorial using constexpr
constexpr long long factorial_compiletime(int n) {
    return (n <= 1) ? 1 : n * factorial_compiletime(n - 1);
}

// Usage
long long r_val = factorial_runtime(10); // Computed at runtime
constexpr long long c_val = factorial_compiletime(10); // Computed at compile time! 🎉

// Traditional TMP (more complex)
template<int N>
struct Factorial {
    static const long long value = N * Factorial<N - 1>::value;
};
template<>
struct Factorial<0> {
    static const long long value = 1;
};
// const long long c_val_tmp = Factorial<10>::value;
```
-   **Template Specialization for Performance**: Provide different implementations of a template for specific types to optimize for those types.

---
layout: center
class: text-center
---

# Template Errors Be Like... 🤯

<img src="/memes/meme_template_error_wall.txt" alt="Meme: Template errors - A huge wall of cryptic text" class="mx-auto my-4 max-h-60">

*It's a feature, not a bug... right?*
---
layout: default
---

# 🛠️ Modern C++ Performance Features

## Move Semantics (C++11) 🏃‍♂️💨

**Avoiding expensive copies by "moving" resources.**

-   **Rvalue References (`&&`)**: References that can bind to temporary objects (rvalues).
    -   Allows functions to differentiate between lvalues (objects with names) and rvalues (temporaries).
-   **Move Constructor & Move Assignment Operator**:
    -   "Steal" resources (e.g., dynamically allocated memory) from temporary objects instead of copying them.
    -   Crucial for types that own resources, like `std::vector`, `std::string`, `std::unique_ptr`.
    ```cpp
    class MyString {
    public:
        // Move Constructor
        MyString(MyString&& other) noexcept : data(other.data), size(other.size) {
            other.data = nullptr; // Leave the source object in a valid but empty state
            other.size = 0;
        }
        // ... other members, constructors, assignment operators ...
    private:
        char* data;
        size_t size;
    };
    ```
-   **`std::move`**: Unconditionally casts an lvalue to an rvalue, enabling move operations.
    -   Use with care: after `std::move`, the original object should be considered "empty" or in a valid but unspecified state.
-   **Perfect Forwarding (`std::forward`)**: Forward arguments with their original value categories (lvalue/rvalue) in template functions. Essential for generic factories or wrappers.
-   **Performance Benefit**: Significantly reduces overhead for returning large objects by value, inserting into containers, etc.

---
layout: default
---

# Standard Library Optimizations 📚

**The C++ Standard Library is your friend for performance!**

-   **Algorithm Selection**:
    -   `std::sort` vs. `std::stable_sort`.
    -   `std::find` vs. `std::lower_bound` (on sorted ranges).
    -   Use algorithms like `std::copy`, `std::transform`, `std::accumulate` – they are often highly optimized (e.g., using SIMD internally).
-   **Parallel Algorithms (C++17)**: Many algorithms now have parallel execution policies.
    ```cpp
    #include <vector>
    #include <algorithm>
    #include <execution> // Required for execution policies

    std::vector<int> data = { ... };
    // Sort in parallel
    std::sort(std::execution::par, data.begin(), data.end());
    ```
    -   Can provide significant speedups on multi-core CPUs for large datasets.
-   **String Optimizations**:
    -   **Small String Optimization (SSO)**: Many `std::string` implementations store small strings directly within the string object, avoiding heap allocations.
    -   `std::string_view` (C++17): A non-owning reference to a string. Great for passing strings without copying, but be careful with lifetimes!
-   **Container Improvements**:
    -   `emplace_back`, `emplace` methods construct objects in-place, avoiding temporary copies/moves.
    -   `reserve` for `std::vector` and `std::string` to pre-allocate memory.
    -   `std::unordered_map`/`std::unordered_set` provide average O(1) lookups.

---
layout: default
---

# Compiler Optimizations 🤖

**Let the compiler do the heavy lifting!**

Compilers are incredibly smart at optimizing code. Trust them, but verify.

-   **Optimization Flags**:
    -   `-O1`: Basic optimizations.
    -   `-O2`: Most common level, good balance of optimization and compile time. (Often default for release builds)
    -   `-O3`: More aggressive optimizations (e.g., auto-vectorization, more inlining). Can sometimes make code slower or larger.
    -   `-Os`: Optimize for size.
    -   `-Ofast`: `-O3` plus optimizations that might violate strict standard compliance (e.g., fast math). Use with caution!
    -   `-g`: Include debug symbols. Can be used with optimization flags.
-   **Link-Time Optimization (LTO) / Whole Program Optimization**:
    -   Allows compiler to optimize across translation units (source files).
    -   Flags: `-flto` (GCC/Clang), `/GL` (MSVC).
    -   Can significantly improve performance by enabling more inlining and interprocedural optimizations.
    -   Increases link time.
-   **Profile-Guided Optimization (PGO)**:
    1.  Compile with PGO instrumentation (`-fprofile-generate` or `/GENPROFILE`).
    2.  Run your application with typical workloads to gather data.
    3.  Recompile with PGO feedback (`-fprofile-use` or `/USEPROFILE`).
    -   Compiler uses runtime data to make better optimization decisions (e.g., branch prediction, inlining).
-   **Compiler-Specific Hints/Intrinsics**:
    -   `__builtin_expect` (GCC/Clang) or `[[likely]]`/`[[unlikely]]` (C++20).
    -   Intrinsics for SIMD, bit manipulation, etc.

**Always measure the impact of different flags!** What works best can depend on your specific codebase and hardware.
---
layout: default
---

# 📊 Measuring & Benchmarking

## Benchmarking Best Practices ⏱️

**If you don't measure, you're just guessing!**

-   **Use a Benchmarking Library**:
    -   **Google Benchmark**: Industry standard, robust, handles many pitfalls.
    -   Others: Hayai, Celero, Nonius.
    -   Avoid manual timing loops (prone to errors: clock resolution, dead code elimination, etc.).
-   **Benchmark the Right Thing**: Isolate the code you want to measure.
-   **Run Multiple Iterations**: Account for JIT, caching effects, and system noise. Libraries do this automatically.
-   **Statistical Significance**: Ensure observed differences are real, not just noise. Run benchmarks long enough.
-   **Stable Environment**: Minimize background processes. Consistent hardware.
-   **Beware of Compiler Optimizations**: Compilers might optimize away code if its result isn't used. Benchmarking libraries help prevent this.
-   **Baseline**: Always compare against a baseline (e.g., the unoptimized version).
-   **Benchmark Realistic Scenarios**: Use representative data and workloads.

```cpp
#include <benchmark/benchmark.h>
#include <vector>
#include <numeric> // For std::iota

// Example function to benchmark
void CreateAndFillVector(int size) {
    std::vector<int> v;
    v.reserve(size); // Pre-allocate
    for (int i = 0; i < size; ++i) {
        v.push_back(i);
    }
    // Make sure the vector is used to prevent optimization
    benchmark::DoNotOptimize(v.data());
}

// Define a benchmark
static void BM_VectorCreation(benchmark::State& state) {
    for (auto _ : state) { // Loop managed by the library
        CreateAndFillVector(state.range(0));
    }
}
// Register the function as a benchmark
BENCHMARK(BM_VectorCreation)->Range(1<<10, 1<<20); // Test with various sizes

// To run (after compiling with benchmark library): ./my_benchmark

// --- Another Example from issue ---
// static void BM_VectorPushBack(benchmark::State& state) {
//     for (auto _ : state) {
//         std::vector<int> v;
//         // state.PauseTiming(); // Example if setup is expensive
//         // setup_data(v, state.range(0));
//         // state.ResumeTiming();
//         for (int i = 0; i < state.range(0); ++i) {
//             v.push_back(i);
//         }
//         benchmark::ClobberMemory(); // Ensure memory ops are not optimized away
//     }
// }
// BENCHMARK(BM_VectorPushBack)->Range(8, 8<<10);
```

---
layout: default
---

# Performance Analysis Tools 🔍

**Beyond profilers: understanding the "why".**

-   **Profilers (Recap)**:
    -   *Time Profilers*: Valgrind (Callgrind), Perf, gprof, Visual Studio Profiler, Xcode Instruments.
        - Show where time is spent.
    -   *Memory Profilers*: Valgrind (Massif), ASan/LSan (AddressSanitizer/LeakSanitizer), Visual Studio Memory Usage.
        - Track allocations, detect leaks, heap usage.
-   **Flame Graphs**:
    -   Visualizes profiler output (especially CPU stack traces).
    -   Width of a function block indicates time spent in it (or its children).
    -   Great for quickly identifying hot paths.
    -   Generated from `perf` data or other profilers.
-   **Cache Profilers**: Valgrind (Cachegrind), Intel VTune.
    -   Simulate cache behavior, show miss rates.
-   **Static Analysis Tools**:
    -   Clang Static Analyzer, Cppcheck, SonarQube, Coverity.
    -   Can find potential performance issues (e.g., inefficient patterns) without running the code.
-   **Compiler Optimization Reports**:
    -   GCC/Clang: `-fopt-info-all` (or more specific like `-fopt-info-vec`).
    -   Shows what optimizations the compiler applied (or failed to apply).

<!-- TODO: Add a profiler comparison chart placeholder -->
<!-- Visual: A simple table comparing features of Perf, Valgrind, VTune -->

---
layout: center
class: text-center
---

# Optimizing for 3 Hours... 🕒

<img src="/memes/meme_optimize_3hrs_for_2ms.txt" alt="Meme: Optimize 3hrs for 2ms - Stonks with tiny arrow" class="mx-auto my-4 max-h-60">

**Hey, every millisecond counts... right? ...Right?** 🤔

*(Sometimes the biggest win is knowing when to stop)*
---
layout: default
---

# 🎯 Real-World Case Studies

## Case Study 1: String Processing 📝

**Scenario**: Optimizing a log parser that processes millions of lines.

**Initial Problem**: Profiler shows excessive time in string splitting, copying, and searching. High memory churn from temporary string objects.

**Before Optimization (Conceptual Code)**:
```cpp
// std::string line; while (getline(log_file, line)) {
//     std::vector<std::string> parts = split(line, ','); // Frequent allocations
//     if (parts[2].find("ERROR") != std::string::npos) { // String searching
//         // process error...
//     }
// }
```

**Optimization Techniques Applied**:
1.  **`std::string_view` (C++17)**: Used for splitting and searching to avoid creating substrings.
2.  **Optimized Splitting Logic**: Custom delimiter search instead of general-purpose `split` that creates many strings.
3.  **Pre-allocation**: If results are stored, `reserve` space in vectors.
4.  **Reduced Copying**: Pass `string_view` by value, ensure functions don't unintentionally copy.
5.  **Regex Optimization**: If using regex, pre-compile patterns, choose efficient engines.

**After Optimization (Conceptual Code)**:
```cpp
// std::string line; while (getline(log_file, line)) {
//     std::string_view line_view(line);
//     // Iterate over views of substrings
//     for (std::string_view part : split_view(line_view, ',')) {
//         if (part.find("ERROR") != std::string_view::npos) { // Faster search on view
//             // process error...
//         }
//     }
// }
```

**Performance Measurements**:
-   Significant reduction in CPU time (e.g., 2x-5x faster).
-   Drastic decrease in memory allocations and overall memory usage.

**Lessons Learned**: For string-heavy tasks, minimizing allocations and copies via `string_view` and careful algorithm choice is key.

---
layout: default
---

# Case Study 2: Mathematical Computations 🧮

**Scenario**: Speeding up a physics simulation involving large vector and matrix operations.

**Initial Problem**: Core simulation loop is slow. Profiler points to expensive matrix multiplications and vector dot products.

**Before Optimization (Conceptual Code)**:
```cpp
// Matrix multiply: C[i][j] = sum(A[i][k] * B[k][j])
// for (int i = 0; i < N; ++i)
//   for (int j = 0; j < N; ++j)
//     for (int k = 0; k < N; ++k)
//       C[i][j] += A[i][k] * B[k][j]; // Cache inefficient access for B
```

**Optimization Techniques Applied**:
1.  **SIMD Vectorization**: Manually using intrinsics or ensuring auto-vectorization for vector/matrix math.
2.  **Loop Optimizations**:
    -   **Tiling/Blocking**: For matrix multiplication to improve cache locality.
    -   **Loop Unrolling**: For small, fixed-size vector operations.
3.  **Algorithm Improvement**: Switched from naive matrix multiplication to Strassen algorithm for very large matrices (if applicable).
4.  **Data Layout**: Ensured data (vectors, matrices) is stored contiguously and aligned for SIMD.
5.  **Reduce Precision**: If acceptable, use `float` instead of `double`.

**After Optimization (Conceptual - Tiled Matrix Multiplication)**:
```cpp
// for (int i0 = 0; i0 < N; i0 += TILE_SIZE)
//   for (int j0 = 0; j0 < N; j0 += TILE_SIZE)
//     for (int k0 = 0; k0 < N; k0 += TILE_SIZE)
//       // Mini-matrix multiplication on tiles
//       for (int i = i0; i < std::min(i0 + TILE_SIZE, N); ++i)
//         for (int j = j0; j < std::min(j0 + TILE_SIZE, N); ++j)
//           for (int k = k0; k < std::min(k0 + TILE_SIZE, N); ++k)
//             C[i][j] += A[i][k] * B[k][j]; // Better cache behavior
```
**Benchmark Results**:
-   Substantial speedup in core calculations (e.g., 3x-10x faster).
-   Compiler reports showing successful vectorization.

**Lessons Learned**: Mathematical code benefits immensely from cache locality, vectorization, and choosing appropriate algorithms for the scale of data.

---
layout: default
---

# Case Study 3: Data Processing Pipeline 🏭

**Scenario**: A multi-stage data ingestion and transformation pipeline is too slow and uses too much memory.

**Initial Problem**: Data copied between stages, inefficient data structures, single-threaded processing.

**Before Optimization (Conceptual)**:
-   Stage 1: Reads data, stores in `std::list<DataObject>`.
-   Stage 2: Filters data, creates new `std::list<DataObject>`.
-   Stage 3: Transforms data, creates final `std::vector<ResultObject>`.
-   Each stage copies data to the next.

**Optimization Techniques Applied**:
1.  **Memory Optimization**:
    -   Switched from `std::list` to `std::vector` where random access or cache locality was beneficial.
    -   Used `std::vector::reserve` to pre-allocate.
    -   Employed `std::move` semantics to transfer data between stages without copying.
    -   Object pooling for `DataObject` if construction/destruction was expensive.
2.  **Parallel Processing**:
    -   Used `std::async` or a thread pool to process data chunks in parallel for independent tasks.
    -   C++17 parallel algorithms (`std::transform`, `std::for_each` with `std::execution::par`).
3.  **Cache Optimization**:
    -   Re-ordered data processing steps to improve temporal and spatial locality.
    -   Ensured data processed in chunks that fit well within CPU caches.
4.  **Algorithmic Changes**: Replaced slow search algorithms with `std::unordered_map` for O(1) average lookups where applicable.

**After Optimization (Conceptual)**:
-   Stage 1: Reads data into `std::vector<DataObject>`, `reserve`d.
-   Stage 2: Filters data in-place or moves to new `std::vector`, leveraging parallel algorithms.
-   Stage 3: Transforms data, `emplace_back`ing into final `std::vector`, also parallelized.

**Results Visualization**:
-   [Placeholder for a chart showing throughput increase and memory usage decrease]
-   Significant reduction in end-to-end processing time.
-   Lower peak memory usage.

**Lessons Learned**: Data pipelines benefit from minimizing copies (move semantics), choosing appropriate data structures for each stage, and leveraging parallelism.
---
layout: default
---

# ⚠️ Common Pitfalls & Anti-Patterns

## Optimization Myths 🚫

**Beliefs that can lead you astray in the quest for speed.**

-   **Myth**: "Writing everything in assembly is the fastest."
    -   **Reality**: Modern compilers often generate better assembly than average humans. Focus on high-level algorithmic and data structure choices. Assembly is rarely needed.
-   **Myth**: "Shorter code is faster code."
    -   **Reality**: Readability and maintainability are important. Obscure tricks for minor gains can backfire. Sometimes more verbose code (e.g., loop unrolling by hand) can be faster, but let the compiler try first.
-   **Myth**: "I know exactly where the bottleneck is."
    -   **Reality**: Intuition is often wrong. Always profile! 📊
-   **Myth**: "Optimize everything, all the time."
    -   **Reality**: Focus on the critical 3% (Knuth). Most code is not performance-critical. Over-optimizing non-bottlenecks wastes time and can make code harder to maintain.
-   **Myth**: "`std::endl` is just like `\n`."
    -   **Reality**: `std::endl` also flushes the stream, which can be very slow, especially in loops. Use `\n` for newlines unless you explicitly need to flush. `cout << "Hello\n";`
-   **Myth**: "More threads always means more speed."
    -   **Reality**: Amdahl's Law. Thread creation, synchronization (mutexes, atomics), and communication have overhead. Too many threads can lead to contention and degrade performance.

**When NOT to Optimize**:
-   When the code is already "fast enough" for its requirements.
-   When the code is not a bottleneck (profile first!).
-   When optimizations make the code unreadable or unmaintainable for negligible gain.
-   During initial development (get it right, then make it fast if needed).

---
layout: default
---

# Common Mistakes 😱

**Frequent blunders in performance optimization.**

1.  **Premature Optimization**: Optimizing before profiling or without clear goals. Wastes time, increases complexity.
2.  **Micro-Optimizations that Hurt**:
    -   Making code unreadable for tiny, unverified gains.
    -   Fighting the compiler: Trying to outsmart a modern optimizing compiler on low-level details often fails.
3.  **Ignoring Algorithmic Complexity**: Focusing on small tweaks when the underlying algorithm is inefficient (e.g., O(n²) instead of O(n log n)).
4.  **Not Using Standard Library Features**: Re-inventing the wheel (e.g., custom sort) poorly when `std::sort` is available and highly optimized.
5.  **Platform-Specific Assumptions Without Checks**: Assuming what's fast on one CPU/OS is fast everywhere.
6.  **Pessimization**: Accidentally making code slower by trying to optimize it. (e.g., manual loop unrolling that thrashes the instruction cache).
7.  **Forgetting to Measure**: Making changes without benchmarking to see if they actually improved things.
8.  **Cargo Cult Optimization**: Applying an optimization seen elsewhere without understanding if it applies to the current problem.
9.  **Excessive Use of `std::shared_ptr`**: When `std::unique_ptr` or references would suffice. Reference counting has overhead.

---
layout: center
class: text-center
---

# "Premature optimization is the root of all evil." - Donald Knuth

<img src="/memes/meme_premature_optimization_evil.txt" alt="Meme: Premature optimization evil - Devil emoji" class="mx-auto my-4 max-h-60">

**Seriously, profile first!** 🙏
---
layout: default
---

# 🔮 Future & Advanced Topics

## C++20/23 Performance Features 🆕

**The latest C++ standards continue to bring performance-relevant features.**

-   **Concepts (C++20)**:
    -   While primarily for improved template error messages and generic code clarity, concepts can indirectly aid performance by enabling more precise trait-based dispatch and potentially better optimization choices by the compiler for constrained templates.
    -   Cleaner interfaces can make it easier to reason about and optimize code.
-   **Coroutines (C++20)**:
    -   Provide a framework for resumable functions. Useful for asynchronous operations, generators, etc.
    -   **Overhead**: There is some overhead associated with creating and managing coroutine frames. May not be suitable for extremely performance-critical, low-latency paths if a simpler state machine would suffice. However, they can simplify complex async logic, potentially leading to more maintainable and thus optimizable code.
-   **Modules (C++20)**:
    -   **Compilation Speed**: Significantly improve build times by replacing textual inclusion of headers with binary module interfaces. Faster builds mean faster iteration cycles for development and benchmarking.
    -   **Potential for Optimization**: Better isolation and understanding of translation units *might* offer new avenues for inter-module optimization in the future.
-   **`std::span` (C++20)**:
    -   A non-owning view over a contiguous sequence of objects.
    -   Encourages passing views of data rather than pointers and sizes, or copying into new containers.
    -   Reduces risk of errors, can improve clarity, and avoids unnecessary copies, which is a performance win.
-   **`std::format` (C++20)**:
    -   Modern, fast, and safe text formatting library. Generally more performant than iostreams and often safer than `sprintf`.
-   **Further C++23 Features**: (e.g., `std::mdspan` for multi-dimensional views, more `constexpr` functions) continue this trend of providing safer, more expressive, and often more performant tools.

---
layout: default
---

# Parallel & Concurrent Programming 🔀

**Leveraging multi-core processors for higher throughput.**

-   **Thread Management**:
    -   `std::thread`, `std::jthread` (C++20 - RAII wrapper for threads).
    -   Thread pools for managing a fixed number of worker threads, reducing creation/destruction overhead.
-   **Task-Based Parallelism**: `std::async`, `std::packaged_task`, `std::promise`/`std::future`.
    -   Higher-level abstractions for running tasks asynchronously.
-   **Lock-Free Programming**:
    -   Designing data structures and algorithms that don't require traditional mutexes.
    -   Uses atomic operations (`std::atomic<T>`).
    -   Extremely complex to get right, but can offer significant scalability in specific scenarios.
    -   Susceptible to subtle bugs (ABA problem, memory ordering issues).
-   **Atomic Operations (C++11 and later)**:
    -   `std::atomic<T>` provides atomic read, write, read-modify-write operations.
    -   Essential for lock-free programming and fine-grained synchronization.
-   **Memory Ordering**: (`std::memory_order`)
    -   Controls how atomic operations synchronize memory with respect to other threads.
    -   `memory_order_relaxed`, `memory_order_acquire`, `memory_order_release`, `memory_order_acq_rel`, `memory_order_seq_cst`.
    -   Crucial for correctness in lock-free code. Using stricter ordering than necessary can incur performance penalties.
-   **Standard Parallel Algorithms (C++17)**: As mentioned before, use `std::execution::par` and `std::execution::par_unseq`.

**Challenges**: Race conditions, deadlocks, data sharing complexity, scalability bottlenecks.

---
layout: default
---

# Hardware-Specific Optimizations 🖥️

**Going deep: tailoring code to the metal.** (Use sparingly and with profiling!)

-   **CPU Architecture Awareness**:
    -   Different CPUs have different instruction sets, cache sizes, pipeline depths, branch predictors.
    -   Compilers can target specific microarchitectures (e.g., `-march=native` for GCC/Clang to optimize for the build machine's CPU).
    -   Be aware of instruction latencies and throughputs for your target CPU.
-   **SIMD Intrinsics (Manual Vectorization)**:
    -   SSE, AVX, AVX2, AVX-512 (Intel/AMD). NEON (ARM).
    -   Gives direct control over vector instructions. Powerful but less portable.
-   **Memory Hierarchy Optimization**:
    -   **NUMA (Non-Uniform Memory Access)**: In multi-socket systems, accessing memory local to a CPU core is faster. Pin threads to cores and allocate memory on the local NUMA node.
    -   **Memory Bandwidth**: Consider how your data access patterns utilize available memory bandwidth.
    -   **Prefetching**: Manually hint to the CPU to fetch data into cache before it's needed (`__builtin_prefetch` on GCC/Clang). Compilers often do this, but manual prefetching can help in specific cases.
-   **GPU Computing (GPGPU)**:
    -   Offload massively parallel workloads to GPUs.
    -   **CUDA (NVIDIA)**, **OpenCL (Cross-platform)**, **SYCL**, **HIP (AMD)**.
    -   Requires different programming models and careful data management between CPU and GPU.
-   **FPGA / Custom Hardware Accelerators**:
    -   For extreme performance in specific domains, offload tasks to FPGAs or custom ASICs.
    -   Highly specialized.

**Portability vs. Performance**: Hardware-specific optimizations often reduce portability. Use conditional compilation (`#ifdef`) or runtime dispatch if supporting multiple architectures.
---
layout: default
---

# 🎓 Conclusion & Resources

## Key Takeaways 📝

**Top 10 Optimization Principles (Checklist Style!):**

1.  ✅ **Measure First, Optimize Second**: Don't guess, profile! 📊
2.  ✅ **Master Big O**: Algorithms & data structures are paramount. 📈
3.  ✅ **Cache is King**: Write cache-friendly code (data locality). 🗄️
4.  ✅ **Reduce Allocations**: Minimize `new`/`delete`; use stack, `reserve`, object pools. ♻️
5.  ✅ **Embrace Move Semantics**: Avoid unnecessary copies. 🏃‍♂️💨
6.  ✅ **Leverage the Standard Library**: Use optimized algorithms & containers. 📚
7.  ✅ **Trust Your Compiler (Usually)**: Enable optimizations (`-O2`/`-O3`, LTO, PGO). 🤖
8.  ✅ **Write Predictable Code**: Help the branch predictor. 🎯
9.  ✅ **Consider Parallelism**: Utilize multiple cores (carefully!). 🔀
10. ✅ **Readability Counts**: Balance performance with maintainable code. ✨

**When to apply?**
- Start with high-level (Algorithms, Data Structures).
- Then move to memory/cache optimizations.
- Advanced techniques (SIMD, TMP, Lock-free) for critical hotspots, with careful measurement.

---
layout: default
---

# Learning Path 🛤️

**Continue your journey to C++ Performance Mastery!**

**Recommended Books 📚:**
-   "Effective Modern C++" by Scott Meyers (Essential for modern C++)
-   "Optimized C++" by Kurt Guntheroth (Practical techniques and case studies)
-   "C++ Concurrency in Action" by Anthony Williams (For parallel/concurrent programming)
-   "Computer Systems: A Programmer's Perspective" by Bryant & O'Hallaron (Understanding hardware impact)

**Online Resources 🌐:**
-   **CppCon, C++Now, Meeting C++ Talks**: (YouTube) - Many talks on performance.
-   **Agner Fog's Optimization Manuals**: In-depth details on CPU architecture and low-level optimization.
-   **Compiler Explorer (godbolt.org)**: See the assembly your C++ code generates.
-   **Stack Overflow**: Search for specific performance questions.
-   **Blogs**: Sutter's Mill, Herb Sutter on C++; Preshing on Programming (Concurrency, Performance).

**Practice Projects 💻:**
-   Optimize a simple game engine.
-   Build a high-performance data processing tool.
-   Contribute to open-source projects focusing on performance.
-   Implement algorithms and data structures from scratch, then benchmark against `std` versions.

**Communities to Join 🤝:**
-   C++ Slack / Discord channels
-   Local C++ User Groups
-   ISO C++ Standards Committee papers (for the very latest developments)

---
layout: two-cols
---

# Q&A ❓

**Ask me anything about C++ Performance!**

<br>
<br>

**Thank You! 🙏**

*Happy Optimizing!* 🚀⚡🔥

<br>
<br>

**Contact / Follow:**
-   Your Name / Handle
-   github.com/your-repo (if you post slides)
-   linkedin.com/in/you

::right::

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="Slidev"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <mdi-github />
  </a>
</div>

<!-- You can add a fun image or a final meme here related to questions or thanking the audience -->
<img src="/memes/meme_compiler_optimization_magic.txt" alt="Meme: Compiler Magic for Q&A" class="mx-auto my-4 max-h-50">
