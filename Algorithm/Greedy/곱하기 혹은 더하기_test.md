~~~python
import unittest
from main import *


class TddTest(unittest.TestCase):

    def testSolution(self):
        self.assertEqual(576, solution('02984'))

    def testSolution2(self):
        self.assertEqual(210, solution('567'))

    def testSolution3(self):
        self.assertEqual(0, solution('000000'))
        self.assertEqual(6, solution('111111'))
        self.assertEqual(576, solution('029840'))


if __name__ == '__main__':
    unittest.main()
~~~
