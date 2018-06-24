#QUESTION
https://leetcode.com/problems/longest-substring-without-repeating-characters/description/

#SOLUTION
'''
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        if s is None:
            return 0
        last_seen_letter_to_index_hash = dict()
        result = 0
        start_index = 0
        for curr_index, letter in enumerate(s):
            if letter in last_seen_letter_to_index_hash:
                last_seen_letter_index = last_seen_letter_to_index_hash[letter]
                diff = curr_index - last_seen_letter_index
                if diff == 1:
                    # Deal with duplicate letters that are next to each other
                    start_index = curr_index
                else:
                    new_start_index = last_seen_letter_index + 1
                    if new_start_index > start_index: 
                        # Only update the start if further up the string
                        start_index = new_start_index
            last_seen_letter_to_index_hash[letter] = curr_index
            new_length = curr_index - start_index + 1
            result = max(result, new_length)
        return result
'''
