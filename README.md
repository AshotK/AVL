 #include<iostream>
    using namespace std;

    struct node{
        int key;
        node *left;
        node *right;
        int height;

        node (int k){
            key  = k;
            left  = NULL;
            right = NULL;
            height = 1;
        }
    };

    class avl{
        public:
            node* root;

            avl(): root (NULL){}

            int height(node *temp) {
            if (temp == NULL)
                return 0;
            else return temp -> height;
            }

            node *Rrotate(node *y){
                node *x = y -> left;
                node *k = x -> right;
                x -> right = y;
                y -> left = k;
                y -> height = max(height(y -> left), height(y -> right)) + 1;
                x -> height = max(height(x -> left), height(x -> right)) + 1;
                return x;
            }

            node *Lrotate(node *x){
                node *k = x -> right;
                node *y = k -> left;
                k -> left = x;
                x -> right = y;
                x -> height = max(height(x -> left), height(x -> right)) + 1;
                k -> height = max(height(k -> left), height(k -> right)) + 1;
                return k;
            }

            int balfactor(node *temp){
                if (temp == NULL)
                    return 0;
                return height(temp -> left) - height(temp -> right);
            }


            node* balance(node* p, int key){
                if( balfactor(p)  == 2 ){
                    if(key > p -> left -> key)
                        p -> left = Lrotate(p -> left);
                    return Rrotate(p);
                }
                if( balfactor(p) == -2 ){
                    if(key < p -> right -> key)
                        p -> right = Rrotate(p -> right);
                    return Lrotate(p);
                }
                return p;
            }

            node* push(node* temp, int key){
                if (temp == NULL)
                    return new node(key);

                if (key < temp -> key)
                    temp -> left  = push(temp -> left, key);
                else {
                    if (key > temp -> key)
                        temp -> right = push(temp -> right, key);
                    else
                        return temp;
                        }
                temp -> height = 1 + max(height(temp -> left), height(temp -> right));
                if (balfactor(temp) == 2 || balfactor(temp) == -2)
                    temp = balance(temp, key);
                return temp;
            }

            node* pop(node* p, int key){
                if( !p ) return 0;
                if( key < p -> key )
                    p -> left = pop(p -> left, key);
                else
                    if( key > p -> key )
                        p -> right = pop(p ->right, key);
                    else{
                        node* q = p->left;
                        node* r = p->right;
                        delete p;
                        if (!r)
                            return q;
                        node* min = findmin (r);
                        min -> right = removemin (r);
                        min -> left = q;
                        return balance (min, min -> key);
                    }
                    return balance (p, p -> key);
                }

                node* removemin(node *p){
                if (p -> left == 0)
                    return p -> right;
                p -> left = removemin (p -> left);
                return balance(p, p -> key);
                }

                node* findmin (node* p){
                if (p-> left) findmin (p-> left);
                else return p;
                }


             node* print(node* temp){
                 if (root){
                    if (temp == root)
                        cout << "root: " << temp -> key << endl;
                    else
                        cout << temp -> key << endl;
                    if( temp -> left ){
                        cout << "left: ";
                        print(temp -> left);
                    }
                    else cout << "left: 0" << endl;
                    if( temp -> right ){
                        cout << "right: ";
                        print(temp -> right);
                    }
                    else cout << "right: 0"<< endl;
                    return 0;
                 }
                 else cout << " really?";
                 return 0;
                }

            node* deleteall(node *temp){
            if (root){
                if(temp -> right -> right)
                    deleteall(temp -> right);
                if(temp -> left -> right)
                    deleteall(temp -> left);
                temp -> right == NULL;
                temp -> left == NULL;
            }
            }

            ~avl(){
                deleteall(root);
                root == NULL;
            }

    };


    int main(){
        avl tree;
        tree.root = tree.push(tree.root, 6);
        tree.root = tree.push(tree.root, 1);
        tree.root = tree.push(tree.root, 10);
        tree.root = tree.push(tree.root, 8);
        tree.root = tree.push(tree.root, 11);
       tree.root = tree.push(tree.root, 7);
        tree.root = tree.push(tree.root, 9);


        /*         8
                 /    \
               6       10
             /   \    /  \
            1     7  9   11    */
        cout << "Your AVL tree is: "<< endl;
        tree.print(tree.root);

        cout << "Your AVL tree is: "<< endl;
        tree.root = tree.pop(tree.root, 6);
        tree.root = tree.pop(tree.root, 1);
        tree.print(tree.root);

        return 0;
    }
