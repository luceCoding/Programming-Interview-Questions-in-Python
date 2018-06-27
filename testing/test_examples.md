```
import unittest

def add_two_nums(num1, num2):
    if not isinstance(num1, int) or not isinstance(num2, int):
        raise TypeError('Parameters are not of int type.')
    return num1 + num2

class TestAddTwoNums(unittest.TestCase):
    
    def test_adding_postives(self):
        result = add_two_nums(1, 2)
        self.assertEqual(result, 3)
        
    def test_adding_big_pos_ints(self):
        result = add_two_nums(99999999999, 99999999999)
        true_result = 99999999999 + 99999999999
        self.assertEqual(result, true_result)
        
    def test_adding_negatives(self):
        result = add_two_nums(-1, -2)
        self.assertEqual(result, -3)
        
    def test_adding_big_pos_ints(self):
        result = add_two_nums(-99999999999, -99999999999)
        true_result = -99999999999 + -99999999999
        self.assertEqual(result, true_result)
        
    def test_invalid_types(self):
        invalid_types = ['two', (), {}, []]
        for each in invalid_types:
            self.assertRaises(TypeError, add_two_nums, 1, each)
            self.assertRaises(TypeError, add_two_nums, each, 1)
            self.assertRaises(TypeError, add_two_nums, each, each)
            
if __name__ == '__main__':
    unittest.main()
```

```
from mock import patch

def foo(request):
    if isRequestOK(request):
        return True
    else:
        return False
    
class TestFoo():
    @patch("application.isRequestOK")
    def testGoodRequest(mock_isRequestOK):
        mock_isRequestOK.return_value = True
        assert_equals(foo('gg'), True)
        
    @patch("application.isRequestOK")
    def testGoodRequest(mock_isRequestOK):
        mock_isRequestOK.return_value = False
        assert_equals(foo('gg'), False)
```
