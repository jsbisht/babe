# Linked List

## Linked List API

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
```

The above API can be used as follows:

```js
function main() {
    var list = new LinkedList();

    // add items to list and display
    list.add(1);
    list.add(2);
    list.add(3);
    console.log(list.size());
    list.print();
    // remove items from list and display
    list.remove(1);
    console.log(list.size());
    list.print();
    // get item
    console.log(list.get(1));
    console.log(list.get(2));
}
```