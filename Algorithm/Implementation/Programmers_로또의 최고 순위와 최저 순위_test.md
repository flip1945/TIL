~~~python
import unittest
from main import *


class TddTest(unittest.TestCase):

    def test_solution(self):
        self.assertEqual([3, 5], solution([44, 1, 0, 0, 31, 25], [31, 10, 45, 1, 6, 19]))
        self.assertEqual([1, 6], solution([0, 0, 0, 0, 0, 0], [38, 19, 20, 40, 15, 25]))
        self.assertEqual([1, 1], solution([45, 4, 35, 20, 3, 9], [20, 9, 3, 45, 4, 35]))

    def test_win_chk(self):
        self.assertEqual(1, win_chk([1, 2, 3, 4, 5, 6], [1, 2, 3, 4, 5, 6]))
        self.assertEqual(6, win_chk([1, 2, 3, 4, 5, 6], [0, 0, 0, 0, 0, 0]))
        self.assertEqual(6, win_chk([0, 0, 0, 0, 0, 0], [1, 2, 3, 4, 5, 6]))
        self.assertEqual(6, win_chk([1, 0, 0, 0, 0, 0], [1, 2, 3, 4, 5, 6]))
        self.assertEqual(5, win_chk([1, 2, 0, 0, 0, 0], [1, 2, 3, 4, 5, 6]))
        self.assertEqual(4, win_chk([1, 2, 3, 0, 0, 0], [1, 2, 3, 4, 5, 6]))
        self.assertEqual(3, win_chk([1, 0, 0, 4, 5, 6], [1, 2, 3, 4, 5, 6]))
        self.assertEqual(2, win_chk([1, 0, 3, 4, 5, 6], [1, 2, 3, 4, 5, 6]))

    def test_high_rank_find(self):
        win_nums = [31, 10, 45, 1, 6, 19]
        self.assertEqual([31, 10, 45, 1, 6, 19, 0], high_rank_find(win_nums))


if __name__ == '__main__':
    unittest.main()
~~~
