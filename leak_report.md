# Leak report

When 'clean_whitespace.c' is compiled and 'clean_whitespace' is
checked for memory leaks with valgrind --leak-check=full, there are, at exit,
46 bytes in 6 blocks, and there are a total of 7 allocations, 1 frees, and 1070
bytes allocated.

valgrind reports that the losses are at calloc, strip, is_clean, and main.

Solution:
In is_clean, we allocate memory to store the stripped str.
We need to free this allocation only if the stripped str
is empty.  This is because, in strip, the calloc, the allocation of memory, happens
after we check for the empty string.