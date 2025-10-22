# Exp No: 7 – Alpha-Beta Pruning in Minimax Search Algorithm for a Simple Tic-Tac-Toe Game

### **Name: Sanjana Sri N**  
### **Register Number: 2305003007**  

---

## **Aim**
To implement **Alpha-Beta Pruning** in the **Minimax Search Algorithm** for a simple **Tic-Tac-Toe** game.


## **Objective**
- Improve the decision-making efficiency of the computer player by reducing the number of evaluated nodes in the game tree.  
- Develop a Tic-Tac-Toe game using Python that incorporates both the **Minimax Algorithm** and **Alpha-Beta Pruning** for optimal move selection.


## **Theory**

### **Minimax Algorithm**
The **Minimax algorithm** is a recursive strategy used in decision-making and game theory.  
It explores all possible moves and their outcomes, building a game tree to determine the best possible move for a player assuming that the opponent also plays optimally.

### **Alpha-Beta Pruning**
**Alpha-Beta Pruning (α–β)** is an optimization technique for the Minimax algorithm.  
It reduces the number of nodes that are evaluated in the game tree by pruning branches that cannot possibly influence the final decision.  

This means the algorithm gives the same result as Minimax but **with fewer computations**, dramatically improving performance.


## **Sample Program**
```python
import time

class Game:
    def __init__(self):
        self.initialize_game()

    def initialize_game(self):
        self.current_state = [['.','.','.'],
                              ['.','.','.'],
                              ['.','.','.']]
        self.player_turn = 'X'  # Player X always starts

    def draw_board(self):
        for i in range(3):
            for j in range(3):
                print('{}|'.format(self.current_state[i][j]), end=" ")
            print()
        print()

    def is_valid(self, px, py):
        if px < 0 or px > 2 or py < 0 or py > 2:
            return False
        elif self.current_state[px][py] != '.':
            return False
        return True

    def is_end(self):
        # Vertical win
        for i in range(3):
            if (self.current_state[0][i] != '.' and
                self.current_state[0][i] == self.current_state[1][i] and
                self.current_state[1][i] == self.current_state[2][i]):
                return self.current_state[0][i]

        # Horizontal win
        for i in range(3):
            if self.current_state[i] == ['X', 'X', 'X']:
                return 'X'
            elif self.current_state[i] == ['O', 'O', 'O']:
                return 'O'

        # Main diagonal win
        if (self.current_state[0][0] != '.' and
            self.current_state[0][0] == self.current_state[1][1] and
            self.current_state[0][0] == self.current_state[2][2]):
            return self.current_state[0][0]

        # Second diagonal win
        if (self.current_state[0][2] != '.' and
            self.current_state[0][2] == self.current_state[1][1] and
            self.current_state[0][2] == self.current_state[2][0]):
            return self.current_state[0][2]

        # Check for empty cells
        for i in range(3):
            for j in range(3):
                if self.current_state[i][j] == '.':
                    return None
        return '.'

    def max_alpha_beta(self, alpha, beta):
        maxv = -2
        px = py = None
        result = self.is_end()

        if result == 'X': return (-1, 0, 0)
        elif result == 'O': return (1, 0, 0)
        elif result == '.': return (0, 0, 0)

        for i in range(3):
            for j in range(3):
                if self.current_state[i][j] == '.':
                    self.current_state[i][j] = 'O'
                    (m, min_i, in_j) = self.min_alpha_beta(alpha, beta)
                    if m > maxv:
                        maxv = m
                        px, py = i, j
                    self.current_state[i][j] = '.'

                    if maxv >= beta:
                        return (maxv, px, py)
                    if maxv > alpha:
                        alpha = maxv
        return (maxv, px, py)

    def min_alpha_beta(self, alpha, beta):
        minv = 2
        qx = qy = None
        result = self.is_end()

        if result == 'X': return (-1, 0, 0)
        elif result == 'O': return (1, 0, 0)
        elif result == '.': return (0, 0, 0)

        for i in range(3):
            for j in range(3):
                if self.current_state[i][j] == '.':
                    self.current_state[i][j] = 'X'
                    (m, max_i, max_j) = self.max_alpha_beta(alpha, beta)
                    if m < minv:
                        minv = m
                        qx, qy = i, j
                    self.current_state[i][j] = '.'

                    if minv <= alpha:
                        return (minv, qx, qy)
                    if minv < beta:
                        beta = minv
        return (minv, qx, qy)

    def play_alpha_beta(self):
        while True:
            self.draw_board()
            self.result = self.is_end()

            if self.result != None:
                if self.result == 'X':
                    print('The winner is X!')
                elif self.result == 'O':
                    print('The winner is O!')
                else:
                    print("It's a tie!")
                self.initialize_game()
                return

            if self.player_turn == 'X':
                while True:
                    start = time.time()
                    (m, qx, qy) = self.min_alpha_beta(-2, 2)
                    end = time.time()
                    print('Evaluation time: {}s'.format(round(end - start, 7)))
                    print('Recommended move: X = {}, Y = {}'.format(qx, qy))

                    px = int(input('Insert the X coordinate: '))
                    py = int(input('Insert the Y coordinate: '))
                    if self.is_valid(px, py):
                        self.current_state[px][py] = 'X'
                        self.player_turn = 'O'
                        break
                    else:
                        print('Invalid move! Try again.')
            else:
                (m, px, py) = self.max_alpha_beta(-2, 2)
                self.current_state[px][py] = 'O'
                self.player_turn = 'X'

def main():
    g = Game()
    g.play_alpha_beta()

if __name__ == "__main__":
    main()
```

## Sample Input and Output

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/8d5e329a-9aff-41a6-bcf0-46efa10e1b92)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/438b242d-54ba-443e-b040-a936e6ae3b55)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/99a33390-fa11-4ade-a19f-e93bcd7aaec9)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/440797bd-53cb-49c1-b18d-89776864c3e7)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/81575a16-26b2-46f1-a8ac-27c9ed0a0fe5)

## PROGRAM
```python
import time
b=[['.']*3 for _ in range(3)]
turn='X'

def show():[print('|'.join(r))for r in b];print()
def win():
    for i in range(3):
        if b[i][0]==b[i][1]==b[i][2] != '.':return b[i][0]
        if b[0][i]==b[1][i]==b[2][i] != '.':return b[0][i]
    if b[0][0]==b[1][1]==b[2][2] != '.':return b[0][0]
    if b[0][2]==b[1][1]==b[2][0] != '.':return b[0][2]
    return '.' if all(b[i][j]!='.'for i in range(3)for j in range(3))else None

def maxv(a,beta):
    r=win()
    if r=='X':return -1,0,0
    if r=='O':return 1,0,0
    if r=='.':return 0,0,0
    v=-2;x=y=0
    for i in range(3):
        for j in range(3):
            if b[i][j]=='.':
                b[i][j]='O';m,_,_=minv(a,beta);b[i][j]='.'
                if m>v:v,x,y=m,i,j
                if v>=beta:return v,x,y
                a=max(a,v)
    return v,x,y

def minv(a,beta):
    r=win()
    if r=='X':return -1,0,0
    if r=='O':return 1,0,0
    if r=='.':return 0,0,0
    v=2;x=y=0
    for i in range(3):
        for j in range(3):
            if b[i][j]=='.':
                b[i][j]='X';m,_,_=maxv(a,beta);b[i][j]='.'
                if m<v:v,x,y=m,i,j
                if v<=a:return v,x,y
                beta=min(beta,v)
    return v,x,y

while 1:
    show();r=win()
    if r:print('X wins!'if r=='X'else'O wins!'if r=='O'else'Tie!');break
    if turn=='X':
        t=time.time();_,x,y=minv(-2,2)
        print(f"Eval:{round(time.time()-t,4)}s Move:X={x},Y={y}")
        x,y=int(input("X:")),int(input("Y:"))
        if 0<=x<3and 0<=y<3and b[x][y]=='.':b[x][y]='X';turn='O'
        else:print("Invalid!")
    else:_,x,y=maxv(-2,2);b[x][y]='O';turn='X'

```

## OUTPUT

<img width="807" height="695" alt="image" src="https://github.com/user-attachments/assets/b60f7f51-5011-4828-a35a-4139bc0aa0bf" />
<img width="630" height="432" alt="image" src="https://github.com/user-attachments/assets/7611ed9c-5642-4c1c-abde-bf2c0e9d6848" />


## RESULT
We have successfully implemented Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game.
