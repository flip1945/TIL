~~~python
import unittest
from main import *


class TddTest(unittest.TestCase):

    def test_solution(self):
        self.assertEqual("<3, 6, 2, 7, 5, 1, 4>", solution(7, 3))

    def test_print_answer(self):
        self.assertEqual("<3, 6, 2, 7, 5, 1, 4>", print_answer([3, 6, 2, 7, 5, 1, 4]))


if __name__ == '__main__':
    unittest.main()
~~~
