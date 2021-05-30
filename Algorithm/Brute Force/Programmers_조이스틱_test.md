~~~python

import unittest
from main import *


class TddTest(unittest.TestCase):

    def test_solution(self):
        self.assertEqual(56, solution("JEROEN"))
        self.assertEqual(0, solution("AAAA"))
        self.assertEqual(4, solution("AAZZ"))
        self.assertEqual(0, solution("A"))
        self.assertEqual(11, solution("ABABAAAAABA"))
        self.assertEqual(0, solution("AAA"))
        self.assertEqual(3, solution("BBA"))

    def test_alphabet_count(self):
        self.assertEqual(0, alphabet_count("A"))
        self.assertEqual(1, alphabet_count("Z"))
        self.assertEqual(9, alphabet_count("J"))
        self.assertEqual(10, alphabet_count("Q"))


if __name__ == '__main__':
    unittest.main()
~~~
