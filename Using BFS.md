import numpy as np

h,w = map(int,input('map의 높이와 너비를 지정하시오 ex) 3,4 -> 세로:4칸, 가로:5칸 \n').split(','))
sy,sx = map(int,input('시작 지점을 지정하시오 ex) 0,0 \n').split(','))
gy,gx = map(int,input('도착 지점을 지정하시오 ex) 3,4 \n').split(','))
non = list(input('지나갈 수 없는 좌표를 지정하시오 ex) 1,2 3,3 \n').split())

def create_map(h,w) :
    m = []
    for i in range(h+1) :
        m.append(list('0'*(w+1)))
    for i in range(len(non)):
        ny, nx = map(int,non[i].strip("'").split(','))
        m[ny][nx] = '#'
    m[sy][sx] = 'S'
    m[gy][gx] ='G'
    return m

m=create_map(h,w)
print('\nmap:')
for i in range(h+1):
    for j in range(w+1) :
        print(m[i][j], end = ' ')
    print()
    
visited =[]
for i in range(h+1) :
    temp = []
    for j in range(w+1) :
        temp.append(0)
    visited.append(temp)

def check(i,j) :
    return 0 <= i <= h and 0 <= j <= w
dx = [0,0,1,-1]
dy = [1,-1,0,0]

def bfs(m) :
    queue = [(sy,sx)]
    while queue :
        v = queue.pop()
        for k in range(4) :
            di = v[0] + dx[k]
            dj = v[1] + dy[k]
            if check(di,dj):
                if m[di][dj] in '0G':
                    if visited[di][dj] == 0 :
                        queue.insert(0, (di,dj))
                        visited[di][dj] = visited[v[0]][v[1]] + 1
                        if m[di][dj] == 'G' :
                            visited[di][dj] = 'G'
                            visited[sy][sx] = 'S'
                            return visited[v[0]][v[1]] + 1
    return -1

g_path=bfs(m)

for i in range(h+1):
    for j in range(w+1) :
        if m[i][j]=='#':
            print('#', end = ' ')
        else:
            print(visited[i][j], end = ' ')
    print()
    
visited[gy][gx]=g_path
mapping=[]

for i in range(h+1) :
    temp = []
    for j in range(w+1) :
        temp.append(0)
    mapping.append(temp)
    
def bfs_coloring(visited) :
    queue = [(gy,gx)]
    while queue :
        v = queue.pop()
        for k in range(4) :
            di = v[0] + dx[k]
            dj = v[1] + dy[k]
            if check(di,dj):
                if visited[di][dj] == int(visited[v[0]][v[1]])-1:
                    queue.insert(0, (di,dj))
                    mapping[di][dj]= '*'
    mapping[sy][sx] = 'S'
    mapping[gy][gx] = 'G'

bfs_coloring(visited)

for i in range(h+1):
    for j in range(w+1) :
        if m[i][j]=='#':
            print('#', end = ' ')
        else:
            print(mapping[i][j], end = ' ')
    print()
