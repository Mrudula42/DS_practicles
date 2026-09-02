Que5.Hashing..
class HashTable:
    def _init_(self, size):  # ✅ Use double underscores here
        self.size = size
        self.table = [[] for _ in range(size)]
Que5.Hashing..
class HashTable:
    def _init_(self, size):  # ✅ Use double underscores here
        self.size = size
        self.table = [[] for _ in range(size)]

    def hashFunc(self, key):
        return key % self.size

    def insert(self, key, value):
        h = self.hashFunc(key)
        for pair in self.table[h]:
            if pair[0] == key:
                pair[1] = value  # Update value if key exists
                return
        self.table[h].append([key, value])  # Otherwise, add new key-value pair

    def search(self, key):
        h = self.hashFunc(key)
        for pair in self.table[h]:
            if pair[0] == key:
                return pair[1]
        return None  # Key not found

    def delete(self, key):
        h = self.hashFunc(key)
        for i, pair in enumerate(self.table[h]):
            if pair[0] == key:
                del self.table[h][i]
                print("Key deleted.")
                return
        print("Key not found.")

    def display(self):
        for i in range(self.size):
            print(f"{i}: {self.table[i]}")


# Example usage
ht = HashTable(10)
ht.insert(12, "Alice")
ht.insert(22, "Bob")
ht.insert(32, "Charlie")

print("\nInitial Table:")
ht.display()

print("\nSearch 22:", ht.search(22))

print("\nAfter deleting key 22:")
ht.delete(22)
ht.display()

Experiment 3: A call center receives incoming calls, and each call is assigned a unique customer ID. The calls are answered in the order they are received. Your task is to simulate the call queue of a call center using a queue data structure.
• addCall(customerID, callTime): Add a call to the queue with the customer ID and the call time (in minutes).
• answerCall(): Answer and remove the first call from the queue.
• viewQueue(): View all calls currently in the queue without removing them.
• isQueueEmpty(): Check if the queue is empty.

class CallCenterQueue:
    def __init__(self):
        self.queue = []   # Initialize an empty list to store calls

    def addCall(self, customerID, callTime):
        """Add a call with customer ID and call time to the queue"""
        call = {"CustomerID": customerID, "CallTime": callTime}
        self.queue.append(call)
        print(f"Call from Customer {customerID} added at {callTime} minutes.")

    def answerCall(self):
        """Answer and remove the first call in the queue"""
        if not self.isQueueEmpty():
            call = self.queue.pop(0)
            print(f"Answered call from Customer {call['CustomerID']} (Call time: {call['CallTime']} mins).")
        else:
            print("No calls in the queue to answer!")

    def viewQueue(self):
        """View all calls currently in the queue"""
        if not self.isQueueEmpty():
            print("\nCurrent Call Queue:")
            for i, call in enumerate(self.queue, start=1):
                print(f"{i}. Customer ID: {call['CustomerID']}, Call Time: {call['CallTime']} mins")
        else:
            print("The call queue is empty.")

    def isQueueEmpty(self):
        """Check if the queue is empty"""
        return len(self.queue) == 0


# ---- Simulation with user input ----
call_center = CallCenterQueue()

while True:
    print("\n--- Call Center Menu ---")
    print("1. Add Call")
    print("2. Answer Call")
    print("3. View Queue")
    print("4. Check if Queue is Empty")
    print("5. Exit")

    choice = input("Enter your choice (1-5): ")

    if choice == "1":
        customerID = input("Enter Customer ID: ")
        callTime = input("Enter Call Time (in minutes): ")
        call_center.addCall(customerID, callTime)

    elif choice == "2":
        call_center.answerCall()

    elif choice == "3":
        call_center.viewQueue()

    elif choice == "4":
        if call_center.isQueueEmpty():
            print("The call queue is empty.")
        else:
            print("The call queue is not empty.")

    elif choice == "5":
        print("Exiting Call Center Simulation. Goodbye!")
        break

    else:
        print("Invalid choice. Please enter a number between 1 and 5.")


Experiment 3: Implement various operations on a binary search tree, such as insertion, deletion, display, and search. Use Python.
# Define a Node class for the BST
class Node:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None


# Define the Binary Search Tree class
class BinarySearchTree:
    def __init__(self):
        self.root = None

    # Insert a new node in BST
    def insert(self, key):
        self.root = self._insert_recursive(self.root, key)

    def _insert_recursive(self, root, key):
        if root is None:
            return Node(key)
        if key < root.key:
            root.left = self._insert_recursive(root.left, key)
        elif key > root.key:
            root.right = self._insert_recursive(root.right, key)
        return root

    # Search for a key in BST
    def search(self, key):
        return self._search_recursive(self.root, key)

    def _search_recursive(self, root, key):
        if root is None or root.key == key:
            return root
        if key < root.key:
            return self._search_recursive(root.left, key)
        return self._search_recursive(root.right, key)

    # Delete a node from BST
    def delete(self, key):
        self.root = self._delete_recursive(self.root, key)

    def _delete_recursive(self, root, key):
        if root is None:
            return root

        if key < root.key:
            root.left = self._delete_recursive(root.left, key)
        elif key > root.key:
            root.right = self._delete_recursive(root.right, key)
        else:
            # Node with only one child or no child
            if root.left is None:
                return root.right
            elif root.right is None:
                return root.left

            # Node with two children: get inorder successor
            temp = self._minValueNode(root.right)
            root.key = temp.key
            root.right = self._delete_recursive(root.right, temp.key)

        return root

    def _minValueNode(self, node):
        current = node
        while current.left is not None:
            current = current.left
        return current

    # Display the BST (Inorder Traversal)
    def inorder(self):
        print("Inorder Traversal:", end=" ")
        self._inorder_recursive(self.root)
        print()

    def _inorder_recursive(self, root):
        if root:
            self._inorder_recursive(root.left)
            print(root.key, end=" ")
            self._inorder_recursive(root.right)


# ---- Menu-driven program ----
bst = BinarySearchTree()

while True:
    print("\n--- Binary Search Tree Menu ---")
    print("1. Insert Node")
    print("2. Delete Node")
    print("3. Search Node")
    print("4. Display (Inorder)")
    print("5. Exit")

    choice = input("Enter your choice (1-5): ")

    if choice == "1":
        key = int(input("Enter value to insert: "))
        bst.insert(key)
        print(f"{key} inserted into BST.")

    elif choice == "2":
        key = int(input("Enter value to delete: "))
        bst.delete(key)
        print(f"{key} deleted from BST (if it existed).")

    elif choice == "3":
        key = int(input("Enter value to search: "))
        result = bst.search(key)
        if result:
            print(f"{key} found in the BST.")
        else:
            print(f"{key} not found in the BST.")

    elif choice == "4":
        bst.inorder()

    elif choice == "5":
        print("Exiting program. Goodbye!")
        break

    else:
        print("Invalid choice. Please enter a number between 1 and 5.")


Que4:Linked list
class Node:
    """Node representing a single student record."""
    def __init__(self, roll_no, name, marks):
        self.roll_no = roll_no
        self.name = name
        self.marks = marks
        self.next = None


class StudentLinkedList:
    """Singly Linked List to manage student records."""
    def __init__(self):
        self.head = None

    def add_student(self, roll_no, name, marks):
        """Add a new student record at the end."""
        new_node = Node(roll_no, name, marks)
        if self.head is None:
            self.head = new_node
        else:
            temp = self.head
            while temp.next:
                temp = temp.next
            temp.next = new_node
        print(f"Student {name} added successfully!")

    def delete_student(self, roll_no):
        """Delete a student record by Roll Number."""
        temp = self.head
        prev = None

        while temp:
            if temp.roll_no == roll_no:
                if prev:
                    prev.next = temp.next
                else:
                    self.head = temp.next
                print(f"Record with Roll No {roll_no} deleted successfully!")
                return
            prev = temp
            temp = temp.next

        print(f"Record with Roll No {roll_no} not found!")

    def update_student(self, roll_no, new_name=None, new_marks=None):
        """Update a student's name or marks."""
        temp = self.head
        while temp:
            if temp.roll_no == roll_no:
                if new_name:
                    temp.name = new_name
                if new_marks is not None:
                    temp.marks = new_marks
                print(f"Record with Roll No {roll_no} updated successfully!")
                return
            temp = temp.next
        print(f"Record with Roll No {roll_no} not found!")

    def search_student(self, roll_no):
        """Search for a student by Roll Number."""
        temp = self.head
        while temp:
            if temp.roll_no == roll_no:
                print(f"Found -> Roll No: {temp.roll_no}, Name: {temp.name}, Marks: {temp.marks}")
                return
            temp = temp.next
        print(f"Record with Roll No {roll_no} not found!")

    def sort_records(self, key="roll_no", ascending=True):
        """Sort records based on Roll Number or Marks."""
        if self.head is None or self.head.next is None:
            return

        # Convert linked list to list for sorting
        nodes = []
        temp = self.head
        while temp:
            nodes.append(temp)
            temp = temp.next

        reverse = not ascending
        if key == "marks":
            nodes.sort(key=lambda x: x.marks, reverse=reverse)
        else:
            nodes.sort(key=lambda x: x.roll_no, reverse=reverse)

        # Rebuild linked list
        self.head = nodes[0]
        temp = self.head
        for node in nodes[1:]:
            temp.next = node
            temp = node
        temp.next = None

        print(f"Records sorted by {key} in {'ascending' if ascending else 'descending'} order!")

    def display(self):
        """Display all student records."""
        if self.head is None:
            print("No records to display!")
            return

        print("\nStudent Records:")
        print("Roll No\tName\tMarks")
        print("-" * 25)
        temp = self.head
        while temp:
            print(f"{temp.roll_no}\t{temp.name}\t{temp.marks}")
            temp = temp.next
        print("-" * 25)


# ---------------- Example Usage ----------------
if __name__ == "__main__":
    s_list = StudentLinkedList()
    s_list.add_student(101, "Amit", 85)
    s_list.add_student(102, "Priya", 92)
    s_list.add_student(103, "Rohan", 78)
    s_list.display()
    s_list.update_student(102, new_marks=95)
    s_list.search_student(103)
    s_list.sort_records(key="marks", ascending=False)
    s_list.display()

    s_list.delete_student(101)
    s_list.display()
