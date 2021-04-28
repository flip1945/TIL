~~~python
import unittest
from main import *


class TddTest(unittest.TestCase):

    def test_solution(self):
        self.assertEqual(5, solution([1, 1, 9, 1, 1, 1], 0))
        self.assertEqual(2, solution([1, 2, 3, 4], 2))
        self.assertEqual(1, solution([5], 0))

    def test_str_to_list(self):
        self.assertEqual([1, 1, 9, 1, 1, 1], str_to_list("1 1 9 1 1 1"))
        self.assertEqual([1, 2, 3, 4], str_to_list("1 2 3 4"))
        self.assertEqual([5], str_to_list("5"))

    def test_que_pop(self):
        self.assertEqual(True, que_pop_chk([4, 3, 2, 1]))
        self.assertEqual(True, que_pop_chk([4, 3, 2, 4]))
        self.assertEqual(False, que_pop_chk([1, 3, 2, 4]))


if __name__ == '__main__':
    unittest.main()
~~~python
