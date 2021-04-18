~~~python
import unittest
from main import *


class TddTest(unittest.TestCase):

    def test_solution(self):
        self.assertEqual("NO", solution("(())())"))
        self.assertEqual("NO", solution("(((()())()"))
        self.assertEqual("YES", solution("(()())((()))"))
        self.assertEqual("NO", solution("((()()(()))(((())))()"))
        self.assertEqual("YES", solution("()()()()(()()())()"))
        self.assertEqual("NO", solution("(()((())()("))

    def test_stack_chk(self):
        self.assertEqual([], stack_chk(["("], ")"))
        self.assertEqual(["(", "("], stack_chk(["("], "("))

    def test_ps_chk(self):
        self.assertEqual(True, valid_ps_chk("(", ")"))
        self.assertEqual(False, valid_ps_chk("(", "("))

    def test_is_empty(self):
        self.assertEqual("YES", is_empty([]))
        self.assertEqual("NO", is_empty(["(", "("]))


if __name__ == '__main__':
    unittest.main()
~~~
