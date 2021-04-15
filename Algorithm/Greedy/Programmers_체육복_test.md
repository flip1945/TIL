~~~python
import unittest
from main import *


class TddTest(unittest.TestCase):

    def test_solution(self):
        self.assertEqual(5, solution(5, [2, 4], [1, 3, 5]))
        self.assertEqual(4, solution(5, [2, 4], [3]))
        self.assertEqual(2, solution(3, [3], [1]))
        self.assertEqual(3, solution(7, [1, 2, 3, 4, 5, 6, 7], [1, 2, 3]))
        self.assertEqual(4, solution(5, [2, 3, 4], [1, 2, 3]))

    def test_lost_students(self):
        self.assertEqual([1, 1, 0, 1, 0, 1], lost_students(5, [2, 4]))

    def test_reserve_chk(self):
        self.assertEqual([1, 1, 1, 1, 1, 1], reserve_chk([1, 1, 0, 1, 0, 1], [1, 3, 5]))


if __name__ == '__main__':
    unittest.main()
~~~
