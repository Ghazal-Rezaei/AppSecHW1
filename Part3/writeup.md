#Testcases
The results of coverage are included as coverageBefore and coverageAfter

##cov1:
I realized, I was only using type_of_record 1 and 3 in my part2 testcases, so to cover 126 to 130 of the original code, I designed a testcase with type_of_record=2; I ran it against the executable file with argv[1][0]=2 which called the gift_card_json function which additionally increased coverage!

##cov2:
In my previous testcases, I only covered cases 0x01 and 0x09 of the animate function; Thus, here I used another opcode namely 0x10 to cover another case of this function. Which also inccreased coverage.

#Fuzzer
I ran AFL for 3 hours because it was run on a linux virtual machine and was not fast. After then, it was able to find 50 unique crashes and 2 unique hangs, one of which worked om my code. The results are visible in AFLresults image.

##fuzzer1
This is one of the crashes which also was caused by overflowing the registers; I tried to solve the problem once and for all by checking the sizes of arg1 and arg2 soon after they are received from user. Which caused another bug in the program which due to program's complexity, I couldn't fix. So I gave up on that and instead checked the arg only in the crashed case!

##fuzzer2
In the print_gift_card_info function, there is a for loop the condition of which depends on number_of_gift_card_records, somehow, when the record_size_in_bytes is negative and the program goes into the animate function, this amount is tampered with and the function never exits the loop!
Again trying to solve the problem by checking record_size_in_bytes value immediately after it's extracted from the file caused another bug in the program. Therefore, I patched it just in the spot that caused the problem.
