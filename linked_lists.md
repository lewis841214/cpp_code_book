# Linked Lists


```

struct LinkedListNode {
    int val;
    LinkedListNode *next;
    LinkedListNode(int val): val (val), next(nullptr) {}
};

int main() {
    LinkedListNode* one = new LinkedListNode(1);
    LinkedListNode* two = new LinkedListNode(2);
    LinkedListNode* three = new LinkedListNode(3);
    one->next = two;
    two->next = three;
    LinkedListNode* head = one;
    
    cout << head->val << endl;
    cout << head->next->val << endl;
    cout << head->next->next->val << endl;
}
LinkedListNode* one = new LinkedListNode(1);    // Allocate node on heap
LinkedListNode* two = new LinkedListNode(2);
LinkedListNode* three = new LinkedListNode(3);

// Linking nodes:
one->next = two;        // one's next pointer points to two's address
two->next = three;      // two's next pointer points to three's address

// The -> operator is shorthand for (*ptr).member
one->next;              // Same as (*one).next
one->next->val;         // Same as (*((*one).next)).val or (*two).val
```
## ListNode Definition
```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};
```

## Reverse Linked List
```cpp
ListNode* reverseList(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    while (curr) {
        ListNode* next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

## Find Middle (Slow-Fast Pointers)
```cpp
ListNode* findMiddle(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}
```

## Detect Cycle
```cpp
bool hasCycle(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) return true;
    }
    return false;
}
```

## Merge Two Sorted Lists
```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    ListNode dummy(0);
    ListNode* curr = &dummy;
    
    while (l1 && l2) {
        if (l1->val <= l2->val) {
            curr->next = l1;
            l1 = l1->next;
        } else {
            curr->next = l2;
            l2 = l2->next;
        }
        curr = curr->next;
    }
    curr->next = l1 ? l1 : l2;
    return dummy.next;
}
```

## Remove Nth Node from End
```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode dummy(0);
    dummy.next = head;
    ListNode* first = &dummy;
    ListNode* second = &dummy;
    
    // Move first n+1 steps ahead
    for (int i = 0; i <= n; i++) {
        first = first->next;
    }
    
    // Move both until first reaches end
    while (first) {
        first = first->next;
        second = second->next;
    }
    
    second->next = second->next->next;
    return dummy.next;
}
```

## Check Palindrome
```cpp
bool isPalindrome(ListNode* head) {
    if (!head || !head->next) return true;
    
    // Find middle
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast->next && fast->next->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    
    // Reverse second half
    ListNode* secondHalf = reverseList(slow->next);
    
    // Compare
    ListNode* firstHalf = head;
    while (secondHalf) {
        if (firstHalf->val != secondHalf->val) return false;
        firstHalf = firstHalf->next;
        secondHalf = secondHalf->next;
    }
    return true;
}
``` 