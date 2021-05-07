~~~python
import unittest
from main import *


class TddTest(unittest.TestCase):

    def test_solution(self):
        nodes = [[5, 3], [11, 5], [13, 3], [3, 5], [6, 1], [1, 3], [8, 6], [7, 2], [2, 2]]
        result = [[7, 4, 6, 9, 1, 8, 5, 2, 3], [9, 6, 5, 8, 1, 4, 3, 2, 7]]

        self.assertEqual(result, solution(nodes))
        self.assertEqual([[1], [1]], solution([[5, 3]]))

    def test_get_root(self):
        nodes = [[5, 3], [11, 5], [13, 3], [3, 5], [6, 1], [1, 3], [8, 6], [7, 2], [2, 2]]
        label_nodes = label(nodes)

        self.assertEqual(6, get_root(label_nodes))

        label_nodes2 = {0: [5, 3], 3: [3, 5], 4: [6, 1], 5: [1, 3], 7: [7, 2], 8: [2, 2]}

        self.assertEqual(3, get_root(label_nodes2))

    def test_get_side_list(self):
        nodes = [[5, 3], [11, 5], [13, 3], [3, 5], [6, 1], [1, 3], [8, 6], [7, 2], [2, 2]]
        label_nodes = label(nodes)

        left, right = get_side_list(6, label_nodes)

        self.assertEqual({0: [5, 3], 3: [3, 5], 4: [6, 1], 5: [1, 3], 7: [7, 2], 8: [2, 2]}, left)
        self.assertEqual({1: [11, 5], 2: [13, 3]}, right)

    def test_label(self):
        nodes = [[5, 3], [11, 5], [13, 3], [3, 5], [6, 1], [1, 3], [8, 6], [7, 2], [2, 2]]
        result_nodes = {0: [5, 3], 1: [11, 5], 2: [13, 3], 3: [3, 5], 4: [6, 1], 5: [1, 3], 6: [8, 6], 7: [7, 2], 8: [2, 2]}
        self.assertEqual(result_nodes, label(nodes))

    def test_make_tree(self):
        root = 6
        nodes = [[5, 3], [11, 5], [13, 3], [3, 5], [6, 1], [1, 3], [8, 6], [7, 2], [2, 2]]
        label_nodes = label(nodes)
        result = [[7], [2], [], [5, 0], [], [8], [3, 1], [4], []]
        result_list = [[] for i in range(len(nodes))]
        self.assertEqual(result, make_tree(root, label_nodes, result_list))

    def test_preorder_dfs(self):
        tree = [[7], [2], [], [5, 0], [], [8], [3, 1], [4], []]
        self.assertEqual([7, 4, 6, 9, 1, 8, 5, 2, 3], preorder_dfs(tree, 6, []))

    def test_postorder_dfs(self):
        tree = [[7], [2], [], [5, 0], [], [8], [3, 1], [4], []]
        self.assertEqual([9, 6, 5, 8, 1, 4, 3, 2, 7], postorder_dfs(tree, 6, []))


if __name__ == '__main__':
    unittest.main()
~~~
