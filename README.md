# Leetcode----2081
Sum of k-Mirror Numbers
//code in java 
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public long kMirror(int k, int n) {
        List<Long> result = new ArrayList<>();
        int len = 1;

        // Generate k-mirror numbers by iterating through possible lengths
        while (result.size() < n) {
            // Generate odd-length palindromes in base-k
            generatePalindromes(result, new char[len], k, n, 0, true);
            if (result.size() == n) break;

            // Generate even-length palindromes in base-k
            generatePalindromes(result, new char[len], k, n, 0, false);
            if (result.size() == n) break;

            len++;
        }

        long sum = 0;
        for (Long num : result) {
            sum += num;
        }
        return sum;
    }

    // Helper function to generate base-k palindromes
    private void generatePalindromes(List<Long> result, char[] arr, int k, int n, int index, boolean isOddLength) {
        if (result.size() == n) {
            return;
        }

        if (index >= (arr.length + (isOddLength ? 1 : 0)) / 2) {
            // Convert the base-k palindrome to base-10
            long number = 0;
            long power = 1;
            for (int i = arr.length - 1; i >= 0; i--) {
                number += (arr[i] - '0') * power;
                power *= k;
            }

            // Check if the base-10 number is a palindrome
            if (isPalindrome(String.valueOf(number))) {
                result.add(number);
            }
            return;
        }

        // Fill the palindrome characters
        for (int i = 0; i < k; i++) {
            if (index == 0 && i == 0 && arr.length > 1) { // Avoid leading zeros for numbers > 1 digit
                continue;
            }
            char c = (char) (i + '0');
            arr[index] = c;
            arr[arr.length - 1 - index] = c; // Mirror character
            generatePalindromes(result, arr, k, n, index + 1, isOddLength);
            if (result.size() == n) return; // Prune if enough numbers are found
        }
    }

    // Helper function to check if a string is a palindrome
    private boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() - 1;
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
