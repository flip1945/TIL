~~~python
import unittest
from main import *


class TddTest(unittest.TestCase):

    def test_solution(self):
        self.assertEqual(["ICN", "JFK", "HND", "IAD"], solution([["ICN", "JFK"], ["HND", "IAD"], ["JFK", "HND"]]))
        self.assertEqual(["ICN", "ATL", "ICN", "SFO", "ATL", "SFO"], solution([["ICN", "SFO"], ["ICN", "ATL"], ["SFO", "ATL"], ["ATL", "ICN"], ["ATL", "SFO"]]))
        self.assertEqual(["ICN", "B", "ICN", "A"], solution([["ICN", "A"], ["ICN", "B"], ["B", "ICN"]]))
        self.assertEqual(['ICN', 'BOO', 'DOO', 'BOO', 'ICN', 'COO', 'DOO', 'COO', 'BOO'], solution([['ICN','BOO' ], [ 'ICN', 'COO' ], [ 'COO', 'DOO' ], ['DOO', 'COO'], [ 'BOO', 'DOO'] ,['DOO', 'BOO'], ['BOO', 'ICN' ], ['COO', 'BOO']]))
        self.assertEqual(["ICN", "COO", "ICN", "COO"], solution([['ICN', 'COO'], ['COO', 'ICN'], ['ICN', 'COO']]))
        self.assertEqual(['ICN', "COO", "ICN", "BOO", "DOO"], solution([["ICN", "COO"], ["ICN", "BOO"], ["COO", "ICN"], ["BOO", "DOO"]]))

    def test_get_airports(self):
        self.assertEqual({"ICN", "SFO", "ATL"}, get_airports([["ICN", "SFO"], ["ICN", "ATL"], ["SFO", "ATL"], ["ATL", "ICN"], ["ATL", "SFO"]]))
        self.assertEqual({"ICN", "HND", "JFK", "IAD"}, get_airports([["ICN", "JFK"], ["HND", "IAD"], ["JFK", "HND"]]))

    def test_create_tickets_dict(self):
        self.assertEqual({'ATL': deque([]), 'ICN': deque([]), 'SFO': deque([])}, create_tickets_dict({"ICN", "SFO", "ATL"}))

    def test_create_airport_table(self):
        self.assertEqual({'ICN': deque(['ATL', 'SFO']), 'ATL': deque(['ICN', 'SFO']), 'SFO': deque(['ATL'])}, create_airport_table({'ATL': deque([]), 'ICN': deque([]), 'SFO': deque([])}, sorted([["ICN", "SFO"], ["ICN", "ATL"], ["SFO", "ATL"], ["ATL", "ICN"], ["ATL", "SFO"]])))


if __name__ == '__main__':
    unittest.main()
~~~
