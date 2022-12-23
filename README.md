# Sleep sort - the lazy man's sort
This repository contains a modern C++ implementation of the Sleep Sort algorithm, with proper comments.

# Code from [main.cpp](main.cpp)
```cpp
#include <iostream>
#include <vector>
#include <thread>
#include <format>

// This is the function that will be run by individual threads
void SleepAndPrint(int n) {
    // replace seconds with minutes for maximum efficiency
    std::this_thread::sleep_for(std::chrono::seconds(n));
    // C++ now has a format() function similar to Python
    std::cout << std::format("{}, ", n);
}

int main() {
    // modify at your own risk
    std::vector<int> nums{1, 4, 10, 7, 2, 18, 3, 0};

    // a vector to store threads
    std::vector<std::thread> threads;

    for (int n: nums) {
        // spawn a new thread that will run SleepAndPrint with arguement n
        std::thread t{SleepAndPrint, n};
        
        // Add thread to the vector
        // threads can only be moved, and not copied
        // we do not join now as it will stall the spawning of new threads
        threads.push_back(std::move(t));
    }

    // now join all the threads
    for (auto &t: threads) {
        // A thread may already be done by the time we get here, so check for that first
        if (t.joinable()) {
            t.join();
        }
    }
    std::cout << "\nSorting complete.";

    return 0;
}
```