import graphviz

class Node:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None

class BinarySearchTree:
    def __init__(self):
        self.root = None

    def insert(self, key):
        self.root = self._insert_recursive(self.root, key)

    def _insert_recursive(self, node, key):
        if node is None:
            return Node(key)
        if key < node.key:
            node.left = self._insert_recursive(node.left, key)
        else:
            node.right = self._insert_recursive(node.right, key)
        return node

    def search(self, key):
        return self._search_recursive(self.root, key)

    def _search_recursive(self, node, key):
        if node is None or node.key == key:
            return node
        if key < node.key:
            return self._search_recursive(node.left, key)
        return self._search_recursive(node.right, key)

    def delete(self, key):
        self.root = self._delete_recursive(self.root, key)

    def _delete_recursive(self, node, key):
        if node is None:
            return node
        if key < node.key:
            node.left = self._delete_recursive(node.left, key)
        elif key > node.key:
            node.right = self._delete_recursive(node.right, key)
        else:
            if node.left is None:
                return node.right
            elif node.right is None:
                return node.left
            min_node = self._min_value_node(node.right)
            node.key = min_node.key
            node.right = self._delete_recursive(node.right, min_node.key)
        return node

    def _min_value_node(self, node):
        current = node
        while current.left is not None:
            current = current.left
        return current

    def visualize(self):
        dot = graphviz.Digraph()
        self._visualize_recursive(dot, self.root)
        dot.render('binary_search_tree.gv', view=True)

    def _visualize_recursive(self, dot, node):
        if node is not None:
            dot.node(str(node.key))
            if node.left is not None:
                dot.edge(str(node.key), str(node.left.key))
                self._visualize_recursive(dot, node.left)
            if node.right is not None:
                dot.edge(str(node.key), str(node.right.key))
                self._visualize_recursive(dot, node.right)

def load_from_file(filename, tree):
    with open(filename, 'r') as file:
        for line in file:
            key = int(line.strip())
            tree.insert(key)

def main():
    tree = BinarySearchTree()
    while True:
        print("\nMenú:")
        print("1. Insertar")
        print("2. Buscar")
        print("3. Eliminar")
        print("4. Cargar desde archivo")
        print("5. Visualizar")
        print("6. Salir")
        choice = input("Seleccione una opción: ")
        if choice == '1':
            key = int(input("Ingrese el valor a insertar: "))
            tree.insert(key)
        elif choice == '2':
            key = int(input("Ingrese el valor a buscar: "))
            if tree.search(key) is not None:
                print("El valor está en el árbol.")
            else:
                print("El valor no está en el árbol.")
        elif choice == '3':
            key = int(input("Ingrese el valor a eliminar: "))
            tree.delete(key)
        elif choice == '4':
            filename = input("Ingrese la ruta del archivo: ")
            load_from_file(filename, tree)
        elif choice == '5':
            tree.visualize()
        elif choice == '6':
            break
        else:
            print("Opción inválida. Intente de nuevo.")

if __name__ == "__main__":
    main()
