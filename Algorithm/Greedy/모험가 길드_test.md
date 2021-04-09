~~~python
import unittest
from main import *


class TddTest(unittest.TestCase):

    def testSolution(self):
        self.assertEqual(2, solution(5, [3, 2, 1, 2, 2]))

    def testSolution2(self):
        self.assertEqual(2, solution(5, [3, 3, 3, 3, 1]))


if __name__ == '__main__':
    unittest.main()
~~~
