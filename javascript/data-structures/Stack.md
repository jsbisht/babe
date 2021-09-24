# Stack

## Stack implementation using Array

```js
function Stack () {
    var stack = [];

    this.push = function(value) {
        stack.push(value);
    }

    this.pop = function() {
        stack.splice(stack.length - 1, 1);
    }

    this.peek = function() {
        return stack[stack.length - 1];
    }

    this.print = function() {
        console.log(stack);
    }

    this.size = function() {
        return stack.length;
    }
}
```

Using the above stack API as follows:

```js
function main() {
    var stack = new Stack();
    stack.push(1);
    stack.push(2);
    stack.push(3);
    console.log(stack.size());
    stack.print();

    stack.pop();
    console.log(stack.size());
    stack.print();

    console.log('Topmost: ' + stack.peek());
}
```

## Stack implementation using Linked List

```js
function Node(next, value) {
    this.next = next;
    this.value = value;
}

// TODO implement iterable
function LinkedList() {
    this.head = null;

    this.add = function(value) {
        if (this.head == null) {
            this.head = new Node(null, value);
        } else {
            var last = this.last();
            last.next = new Node(null, value);
        }
    }

    this.get = function(index) {
        var i = 0;
        var node = this.head;

        if(index == i) {
            return node;
        }
        else if(index < 0) {
            return null;
        }

        while(i++ < index) {
            if(node.next != null) {
                node = node.next;
            }
            else {
                // index out of bound
                return null;
            }
        }

        return node;
    }

    /**
    if first
        set all to null
    if middle
        set previous to next
    if last
        set previous to null
    */
    this.remove = function(index) {
        var i = 0;
        var node = this.head;
        var prev = null;
        
        // if first
        if(index == 0 && node != null) {
            node.next = null;
            node.value = undefined;
            return;
        }
        else if(node == null) return;

        while(node.next != null) {
            // if middle
            if(i == index) {
                prev.next = node.next;
                break;
            }
            
            prev = node;
            node = node.next;
            i++;
        }

        // if last
        if(node.next == null) {
            prev.next = null;
        }

    }

    this.last = function() {
        var node = this.head;
        while (node.next != null) {
            node = node.next;
        }
        return node;
    }

    this.print = function() {
        var items = [];
        var node = this.head;
        if(node == null) {
            console.log('No items in the list'); 
            return; 
        }

        items.push(node.value);
        while (node.next != null) {
            node = node.next;
            items.push(node.value);
        }
        console.log(items);
    }

    this.size = function() {
        var node = this.head;
        var size = 0;
        if(node.next != null) {
            size = 1;
            while (node.next != null) {
                node = node.next;
                size++;
            }
            
        }
        return size;
    }
}

// TODO implement iterable
function Stack() {
    var stack = new LinkedList();
    
    this.push = function(value) {
        stack.add(value);
    }

    this.pop = function() {
        var size = stack.size();
        stack.remove(size);
    }

    this.peek = function() {
        return stack.last().value;
    }

    this.print = function() {
        stack.print();
    }

    this.size = function() {
        return stack.size();
    }
}
```

Using this stack is same as the one based on array:

```js
function main() {
    var stack = new Stack();
    stack.push(1);
    stack.push(2);
    stack.push(3);
    console.log(stack.size());
    stack.print();

    stack.pop();
    console.log(stack.size());
    stack.print();

    console.log('Topmost: ' + stack.peek());
}
```