# 优先队列

```javascript
class PriorityQueue {
    constructor(cmp) {
        this.queue = [];
        this.cmp = cmp;
    }

    offer(e) {
        this.queue.push(e);
    }

    poll() {
        let index = 0;
        for (let i = 0; i < this.queue.length; i++) {
            if (this.cmp(this.queue[i], this.queue[index]) < 0) {
                index = i;
            }
        }
        return this.queue.splice(index, 1)[0];
    }
    size(){
        return this.queue.length;
    }
}

let q = new PriorityQueue((a,b)=>a-b);
q.offer(3);
q.offer(2);
q.offer(1);
q.offer(4);
console.log(q.poll())
console.log(q.poll())
```





# dijkstra算法

```javascript
class State{
    //当前id和到起点的距离
    constructor(id,distFromStart){
        this.id = id;
        this.distFromStart = distFromStart;
    }
}
//每个元素表示一个节点， 比如第一个元素表示节点0到4距离999， 0到1距离5
class Graph {
    constructor(graph) {
        this.graph = graph;
    }

    weight(from, to) {
        return this.graph[from].get(to);
    }

    adj(from) {
        let maps = this.graph[from]
        return Array.from(maps.keys());
    }

    dijkstra(start,end) {
        let v = graph.length;
        let distTo = Array(v).fill(Number.MAX_SAFE_INTEGER);
        distTo[start] = 0;

        let pq = new PriorityQueue((s1,s2)=>s1.distFromStart - s2.distFromStart);
        // 从起点 start 开始进行 BFS
        pq.offer(new State(start,0));
        while(pq.size() > 0){
            let curState = pq.poll();
            let curNodeID = curState.id;
            let curDistFromStart = curState.distFromStart;
            if(curNodeID == end){
                // 带end参数， 提前返回的话， 就不支持负权重了，必须全部路走一遍才能支持负权重。
                return curDistFromStart;
            }
            if(curDistFromStart > distTo[curNodeID]){
                // 已经有一条更短的路径到达 curNode 节点了
                continue;
            }
            // 将curNode的相邻节点装入队列
            for(let nextNodeID of this.adj(curNodeID)){
                let distToNextNode = distTo[curNodeID] + this.weight(curNodeID,nextNodeID);
                if(distTo[nextNodeID] > distToNextNode){
                    // 更新dp table
                    distTo[nextNodeID]= distToNextNode;
                    pq.offer(new State(nextNodeID, distToNextNode));
                }
            }
        }
        // 这里返回是个数组， 包含从start到所有节点的最短路径
        return distTo;
    }
}


let graph = [
    new Map([[4, 999], [1, 5]]),
    new Map([[2, 3]]),
    new Map([[3, 8]]),
    new Map([[4, 7]]),
    new Map()
]
graph = [
    new Map([[1, 4], [2, 5]]),
    new Map(),
    new Map([[1,-3]]),
]

graph = [
    new Map([[1, 10], [2, 7]]),
    new Map([[2,-5]]),
    new Map(),
]

let g = new Graph(graph);
console.log(g.dijkstra(0,2))

```
