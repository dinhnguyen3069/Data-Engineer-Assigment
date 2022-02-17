# DATA ENGINEER QUESTIONS - ANSWER
## SQL Questions
### Q1 - Solution
![image](https://user-images.githubusercontent.com/72053908/154034609-df107b88-bd5f-413d-95c2-f3810c92fa35.png)

Write an SQL to find the second highest score of athletes:

<pre>
SELECT *
FROM Performance
WHERE Score < (Select Max(Score) FROM Performance) 
ORDER BY Score DESC LIMIT 1;
</pre>

### Q2 - Solution
![image](https://user-images.githubusercontent.com/72053908/154034776-8a092c68-d44b-47ca-bbe0-806d9b389e90.png)

Write an SQL query to report all customers who never order anything:

<pre>
SELECT *  
FROM Customers   
WHERE id NOT IN (SELECT customerId FROM Orders);
</pre>

### Q3 - Solution
![image](https://user-images.githubusercontent.com/72053908/154034335-c7be5187-fefe-4be9-8e6a-4bd363b9f322.png)

Write an SQL query to find the employees who are high earners in each of the
departments:

<pre>
SELECT e1.id, e1.name, e1.salary, d.name AS department  
FROM Employee e1  
JOIN Department d ON d.id = e1.departmentId  
WHERE 3 > (SELECT COUNT(DISTINCT e2.salary) FROM Employee e2 WHERE e1.departmentId = e2.departmentId AND e1.salary < e2.salary)  
ORDER BY department DESC, salary DESC;  
</pre>

## Coding questions
### Q4 - Solution

Code python - Find a missing number:  

Explaination:
1. Calculating the sum of this array which contains (n+1) elements from 0 to n  
Sum of consecutive natural numbers formula : Sum = n(n+1)/2  
2. Subtract the values of elements in missing array from Sum we have calculated in step 1  

<pre>
def findMissingnumber(arr, n):
    sum = n*(n+1)/2
    for i in range(len(arr)):
        sum = sum - arr[i]
    return sum
</pre>   

Complexity: O(n)

### Q5 - Solution

Explaination:  
Idea: We will divide 2 arrays to 2 parts (left part and right part) so that amount of elements on each side are equal or more or less than 1 element.  
![image](https://user-images.githubusercontent.com/72053908/154112397-ed29102c-3fad-435d-85d1-cd1231cd3c8d.png)  
Aim: Divide 2 arrays until Max1 <= Min2 and Max2 <= Min1 ---> Max(Max1, Max2) <= Min(Min1, Min2) (1)  
         From (1), we will find the median:  
         If M + N is even number, the median = 0.5*(Max(Max1, Max2) + Min(Min1, Min2)).  
         If M + N is odd number, the median = Min(Min1, Min2) – In this case we will divide 2 arrays to 2 parts so that amount of elements on the left is 1 element less than on the right.  
         If Max1 > Min2, we will decrease i so j will be increased. New index of i will be searched from l to r =  i – 1 by binary search algorithm.     
         If Max2 > Min1, we will increase i so j will be decreased. New index of i will be searched from l = i + 1 to r by binary search algorithm.   
Some notes when updating i and j:  
If i or j < 0, Max1 = -infinity or Max2 = -infinity
If i = M - 1, Min1 = infinity
if j = N - 1, Min2 = infinity

<pre>
def findMedianSortedArrays(nums1, nums2) -> float:
    A, B = nums1, nums2
    total = len(nums1 + nums2)
    half = total//2
    
    if len(A) > len(B):
        A, B = B, A
        
    l, r = 0, len(A) - 1
    while True:
        i = (l + r)//2 
        j = half - i -2
        
        Aleft = A[i] if i >= 0 else float('-infinity')
        Aright = A[i + 1] if (i + 1) < len(A) else float('infinity')
        Bleft = B[j] if j >= 0 else float('-infinity')
        Bright = B[j + 1] if (j + 1) < len(B) else float('infinity')
        
        if Aleft <= Bright and Bleft <= Aright:
            #odd
            if total%2:
                return min(Aright, Bright)
            #even 
            return(max(Aleft, Bleft) + min(Aright, Bright)) / 2
        elif Aleft > Bright:
            r = i - 1 
        else:
            l = i + 1 
</pre>

Complexity: O(log(min(m,n)))  

### Q6 - Solution

Explaination:  
Since the linked list is sorted. We move to the right of the list, and compare the adjacents node. If the adjacents node are same, then remove the second one.  
![image](https://user-images.githubusercontent.com/72053908/154193541-8ff2e8f4-f4ee-4856-803f-76356cdfc077.png)

<pre>
    def deleteDuplicate(self):  
        pointer = self.head  
        pointer_val = pointer.data
         
        while(pointer.next):  
            if(pointer.next.data == pointer_val):  
                pointer.next = pointer.next.next
            else:
                pointer = pointer.next
                pointer_val = pointer.data
</pre>

Complexity: O(n)

## Mathematical statistics + other question  
### Q8 - Solution

I think the best graph to use is Normal Distribution Graph. Because this graph can show the detailed information about group of examinees with each range of score.

### Q9 - Solution

First, we calculate total of outcomes: 6^3 = 216  
If the point obtained in the first roll is greater than the sum of the points obtained in the second roll, the point obtained in the first roll must be 6, 5, 4, 3.

Case 6:  
6 > 5 = 1 + 4 = 2 + 3 = 3 + 2 = 4 + 1 (4C1 cases)    
6 > 4 = 2 + 2 = 1 + 3 = 3 + 1 (3C1 cases)  
6 > 3 = 1 + 2 = 2 + 1 (2C1 cases)  
6 > 2 = 1 + 1 (1C1 cases)  
In the second roll, total of suitable outcomes is (4C1 + 3C1 + 2C1 + 1C1) = 10  

Case 5:  
In the second roll, total of suitable outcomes is (3C1 + 2C1 + 1C1) = 6  

Case 4:  
In the second roll, total of suitable outcomes is (2C1 + 1C1) = 3  

Case 3:  
In the second roll, total of suitable outcomes is 1  

The probability = (1 + 3 + 6 + 10)/216 = 9,26%  


