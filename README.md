from collections import deque
graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': ['F'],
    'F': []
}
def bfs(start):
    visited = set()
    queue = deque([start])
    
    print("BFS Traversal:")
    while queue:
        node = queue.popleft()
        if node not in visited:
            print(node, end=" ")
            visited.add(node)
            queue.extend(graph[node])
bfs('A')




# Graph represented as an adjacency list
graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': ['F'],
    'F': []
}
def dfs(start, visited=None):
    if visited is None:
        visited = set()
        print("DFS Traversal:")
    visited.add(start)
    print(start, end=" ")
    
    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs(neighbor, visited)
dfs('A')



import random
board = [' ']*9
def show_board():
    print(board[0],'|',board[1],'|',board[2])
    print('--+---+--')
    print(board[3],'|',board[4],'|',board[5])
    print('--+---+--')
    print(board[6],'|',board[7],'|',board[8])
def check_winner(symbol):
    wins = [(0,1,2),(3,4,5),(6,7,8),(0,3,6),
            (1,4,7),(2,5,8),(0,4,8),(2,4,6)]
    return any(board[a]==board[b]==board[c]==symbol for a,b,c in wins)
while True:
    show_board()
    move = int(input("Your move (1-9): ")) - 1
    if board[move] != ' ':
        print("Filled! Try again.")
        continue
    board[move] = 'X'
    if check_winner('X'):
        show_board()
        print("You win!")
        break
    if ' ' not in board:
        show_board()
        print("It's a tie!")
        break
    comp = random.choice([i for i in range(9) if board[i] == ' '])
    board[comp] = 'O'
    if check_winner('O'):
        show_board()
        print("Computer wins!")
        break



#8 puzzle problem
from collections import deque
goal = '123456780'   # Goal state (0 is blank)
def move(state, direction):
    i = state.index('0')
    r, c = divmod(i, 3)
    s = list(state)
    if direction == 'up' and r > 0: s[i], s[i-3] = s[i-3], s[i]
    elif direction == 'down' and r < 2: s[i], s[i+3] = s[i+3], s[i]
    elif direction == 'left' and c > 0: s[i], s[i-1] = s[i-1], s[i]
    elif direction == 'right' and c < 2: s[i], s[i+1] = s[i+1], s[i]
    else: return None
    return ''.join(s)
def bfs(start):
    visited = set()
    queue = deque([(start, 0)])  # (state, steps)
    while queue:
        state, steps = queue.popleft()
        if state == goal:
            print(steps)
            return
        visited.add(state)
        for d in ['up', 'down', 'left', 'right']:
            new = move(state, d)
            if new and new not in visited:
                queue.append((new, steps + 1))
bfs('125340678')



# Water Jug Problem
def water_jug(jug1, jug2, target):
    visited = set()
    queue = [(0, 0)] 
    while queue:
        a, b = queue.pop(0)
        if (a, b) in visited:
            continue
        print("(", a, ",", b, ")", sep="")
        visited.add((a, b))
        if a == target or b == target:
            print("Solution found!")
            return
        queue.extend([
            (jug1, b),  # Fill jug1
            (a, jug2),  # Fill jug2
            (0, b),     # Empty jug1
            (a, 0),     # Empty jug2
            (a - min(a, jug2 - b), b + min(a, jug2 - b)),  # Pour jug1 → jug2
            (a + min(b, jug1 - a), b - min(b, jug1 - a))   # Pour jug2 → jug1
        ])
water_jug(4, 3, 2)



from itertools import permutations
n = 4
dist = [
    [0, 10, 15, 20],
    [10, 0, 35, 25],
    [15, 35, 0, 30],
    [20, 25, 30, 0]
]
start = 0
cities = [i for i in range(n) if i != start]
min_path = float('inf')
for perm in permutations(cities):
    current_cost = 0
    k = start
    for j in perm:
        current_cost += dist[k][j]
        k = j
    current_cost += dist[k][start]
    min_path = min(min_path, current_cost)
print("The cost of the most efficient tour =", min_path)



def TowerOfHanoi(n , source, destination, auxiliary):
    if n==1:
        print ("Move disk 1 from source",source,"to destination",destination)
        return
    TowerOfHanoi(n-1, source, auxiliary, destination)
    print ("Move disk",n,"from source",source,"to destination",destination)
    TowerOfHanoi(n-1, auxiliary, destination, source)
n = 4
TowerOfHanoi(n,'A','B','C') 



class MonkeyBananaProblem:
    def __init__(self):
        self.monkey_position = 'A' 
        self.box_position = 'B' 
        self.banana_position = 'C' 
        self.monkey_on_box = False 
    def move_monkey(self, position):
        self.monkey_position = position
        print(f"Monkey moved to {position}")
    def move_box(self, position):
        self.box_position = position
        print(f"Box moved to {position}")
    def climb_box(self):
        if self.monkey_position == self.box_position:
            self.monkey_on_box = True
            print("Monkey climbed on box")
        else:
            self.move_monkey(self.box_position)
            self.climb_box()
    def get_banana(self):
        if self.monkey_on_box and self.monkey_position == self.banana_position:
            print("Monkey got the banana!")
        else:
            print("Monkey can't get banana")
    def solve(self):
        self.move_monkey(self.box_position)
        self.move_box(self.banana_position) 
        self.move_monkey(self.banana_position)
        self.climb_box()
        self.get_banana()
problem = MonkeyBananaProblem()
problem.solve()



# Function to check if a queen can be placed safely
def isSafe(mat, row, col):
    n = len(mat)
    for i in range(row):
        if mat[i][col]:
            return False
    i, j = row - 1, col - 1
    while i >= 0 and j >= 0:
        if mat[i][j]:
            return False
        i -= 1
        j -= 1
    i, j = row - 1, col + 1
    while i >= 0 and j < n:
        if mat[i][j]:
            return False
        i -= 1
        j += 1
    return True
def placeQueens(row, mat, count):
    n = len(mat)
    if row == n:
        count[0] += 1
        print(f"\nSolution {count[0]}:")
        for r in mat:
            print(" ".join(map(str, r)))
        return
    for i in range(n):
        if isSafe(mat, row, i):
            mat[row][i] = 1
            placeQueens(row + 1, mat, count)
            mat[row][i] = 0  # backtrack
def queens():
    n = 8
    mat = [[0] * n for _ in range(n)]
    count = [0]
    placeQueens(0, mat, count)
    return count[0]
if __name__ == "__main__":
    total_solutions = queens()
    print("\nTotal number of solutions for 8-Queens:", total_solutions)



def minimax(depth, nodeIndex, maximizingPlayer, values, alpha, beta): 
    if depth == 3: 
        return values[nodeIndex] 

    if maximizingPlayer: 
        best = float('-inf') 
        for i in range(2): 
            val = minimax(depth + 1, nodeIndex * 2 + i, False, values, alpha, beta) 
            best = max(best, val) 
            alpha = max(alpha, best) 
            if beta <= alpha: 
                break          
        return best      
    else:
        best = float('inf') 
        for i in range(2):          
            val = minimax(depth + 1, nodeIndex * 2 + i, True, values, alpha, beta) 
            best = min(best, val) 
            beta = min(beta, best) 
            if beta <= alpha: 
                break          
        return best 

if __name__ == "__main__":  
    values = [3, 5, 6, 9, 1, 2, 0, -1]  
    print("The optimal value is :", minimax(0, 0, True, values, float('-inf'), float('inf')))
