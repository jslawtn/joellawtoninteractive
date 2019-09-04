<style>
.game-container{
    position: relative;
    max-width: 500px;
    margin: auto;
}

.game-header{
    min-height: 80px;
    text-align: center;
}  

.grid-container{
    margin: auto;
    width:fit-content;
}

.grid{
    display: inline-grid;
    grid-template-columns: auto auto auto;
    grid-column-gap: 5px;
    grid-row-gap: 5px;
}

.btn-node{
    border: none;
}

.node{
    width: 60px;
    height: 60px;
    background-color:cadetblue;
}

.node-player{
    background-color:coral;
}

.node-ai{
    background-color: cornflowerblue;
}
</style>

<template>
    <div class="game-container">
        <div class="game-header">
            <h3>Noughts and Crosses</h3>
            <h6 v-if="gameComplete === true">{{victoryText}}</h6>
        </div>
        <div class="grid-container">
            <div class="grid">
                <div v-for="(node, index) in nodes" :key="index">
                    <button class="node btn-node" v-on:click="playerSelect(node)"
                    :class="{'node-player': node.playerId === -1, 'node-ai': node.playerId === 1}"
                    :disabled="node.playerId !== 0 || gameComplete === true">
                    </button>
                </div>
            </div>
        </div>
        <div v-if="gameComplete === true">
            <button class="btn" v-on:click="restartGame()">Restart</button>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return{
            grid:{
                horizontalNodes: 3,
                verticalNodes: 3
            },
            nodes:[],
            gameComplete: false,
            victoryText: ''
        }
    },
    mounted: function(){
        // Create Board
        for(var x = 0; x < this.grid.horizontalNodes; x++){
            for(var y = 0; y < this.grid.verticalNodes; y++){
                const node = {
                    x: x,
                    y: y,
                    playerId: 0
                }
                this.nodes.push(node);
            }
        }
    },
    methods:{
        restartGame(){
            this.nodes.forEach(x => { x.playerId = 0; });
            this.gameComplete = false;
        },
        playerSelect(node){
            var selectedNode = this.nodes.find(x => x === node);
            selectedNode.playerId = -1;

            if(this.checkGrid(selectedNode, this.nodes) === true){
                this.victoryText = "PLAYER WON";
                this.gameComplete = true;
            }else{
                this.aiSelect();
            }
        },
        aiSelect(){
            var node = this.minMax();

            node.playerId = 1;

            if(this.checkGrid(node, this.nodes) === true){
                this.victoryText = "COMPUTER WON";
                this.gameComplete = true;
            }
        },
        minMax(){
            for(var i = 0; i < this.nodes.length; i ++){
                if(this.nodes[i].playerId === 0){
                    let nodeCopy = {
                        x: this.nodes[i].x,
                        y: this.nodes[i].y,
                        playerId: 1
                    }
                    let gridCopy = JSON.parse(JSON.stringify(this.nodes));

                    gridCopy[i] = nodeCopy;

                    if(this.checkGrid(nodeCopy, gridCopy) === true){
                        return this.nodes[i];
                    }
                }

                if(this.nodes[i].playerId === 0){
                    let nodeCopy = {
                        x: this.nodes[i].x,
                        y: this.nodes[i].y,
                        playerId: -1
                    }
                    let gridCopy = JSON.parse(JSON.stringify(this.nodes));

                    gridCopy[i] = nodeCopy;

                    if(this.checkGrid(nodeCopy, gridCopy) === true){
                        return this.nodes[i];
                    }
                }
            }

            return this.randomNeutralNode();
        },
        checkGrid(node, grid){
            var horizontalPoint = 0;
            var verticalPoint = 0;
            var linearPoint1 = 0;
            var linearPoint2 = 0;
            for(var i = 0; i < grid.length; i++){
                if(grid[i].playerId === node.playerId){
                    if(grid[i].x === node.x){                        
                        horizontalPoint++;
                        if(horizontalPoint === 3){
                            return true; // win
                        }
                    }
                
                    if(grid[i].y === node.y){
                        verticalPoint++;

                        if(verticalPoint === 3){
                            return true; // win
                        }
                    }

                    if(grid[i].x === grid[i].y){
                        linearPoint1++;

                        if(linearPoint1 === 3){
                            return true; // win
                        }
                    }

                    if(grid[i].y === (-1 * grid[i].x) + 2){
                        linearPoint2++;

                        if(linearPoint2 === 3){
                            return true; //win
                        }
                    }
                }
            }
            return false;
        },
        randomNeutralNode(){
            var neutralNodes = this.nodes.filter(x => {
                return x.playerId === 0;
            });

            const maxIndex = neutralNodes.length;
            const randomIndex = Math.floor(Math.random() * Math.floor(maxIndex));

            return neutralNodes[randomIndex];
        }
    }
}
</script>
