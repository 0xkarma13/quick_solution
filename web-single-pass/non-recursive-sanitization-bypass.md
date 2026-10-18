This challenge relied on two logic flaws: 1) the initial filter only blocked the exact lowercase substring admin, while the input was later casefold()’d — so using uppercase (ADMIN) bypassed it. 2) The username length was calculated from the raw input (before sanitization) but compared against the result after replace('admin','') — meaning the length could be controlled independently of the final content.


With the input AADMINDADMINMADMININ (exactly 20 characters), I nested three overlapping admin substrings so that when replace() removed all of them, the leftover characters (scattered across those words) recombined into admin again.

just in username i enterd AADMINDADMINMADMININ and after code became admin    (main)
You just needed to read the code.
and than thinking 

This pattern has a known name: single-pass / non-recursive sanitization bypass (the filter runs only once and never re-checks its own output) — similar to the classic <scr<script>ipt> trick for bypassing XSS filters. In this specific challenge it was intentional, part of the CTF design rather than an accidental bug.
