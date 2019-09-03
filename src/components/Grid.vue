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
                    :class="{'node-player': node.playerId === 0, 'node-ai': node.playerId === 1}"
                    :disabled="node.active === true || gameComplete === true">
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
        for(var x = 0; x < this.grid.horizontalNodes; x++){
            for(var y = 0; y < this.grid.verticalNodes; y++){
                const node = {
                    x: x,
                    y: y,
                    active: false,
                    playerId: undefined
                }
                this.nodes.push(node);
            }
        }
    },
    methods:{
        restartGame(){
            this.nodes.forEach(x => {
                x.active = false;
                x.playerId = undefined;
            });
            this.gameComplete = false;
        },
        playerSelect(node){
            var selectedNode = this.nodes.find(x => x === node);
            selectedNode.active = true;
            selectedNode.playerId = 0;

            if(this.checkGrid(selectedNode) === true){
                this.victoryText = "PLAYER WON";
                this.gameComplete = true;
            }else{
                this.aiSelect();
            }
        },
        aiSelect(){
            var nonActiveNodes = this.nodes.filter(x => {
                return x.active === false;
            });

            const maxIndex = nonActiveNodes.length;

            const randomIndex = Math.floor(Math.random() * Math.floor(maxIndex));

            nonActiveNodes[randomIndex].active = true;
            nonActiveNodes[randomIndex].playerId = 1;

            if(this.checkGrid(nonActiveNodes[randomIndex]) === true){
                this.victoryText = "COMPUTER WON";
                this.gameComplete = true;
            }
        },
        checkGrid(node){
            var horizontalPoint = 0;
            var verticalPoint = 0;
            var linearPoint1 = 0;
            var linearPoint2 = 0;

            for(var i = 0; i < this.nodes.length; i++){
                
                if(this.nodes[i].playerId === node.playerId){
                    if(this.nodes[i].x === node.x){
                        horizontalPoint++;

                        if(horizontalPoint === 3){
                            return true; // win
                        }
                    }
                
                    if(this.nodes[i].y === node.y){
                        verticalPoint++;

                        if(verticalPoint === 3){
                            return true; // win
                        }
                    }

                    if(this.nodes[i].x === this.nodes[i].y){
                        linearPoint1++;

                        if(linearPoint1 === 3){
                            return true; // win
                        }
                    }

                    if(this.nodes[i].y === (-1 * this.nodes[i].x) + 2){
                        linearPoint2++;

                        if(linearPoint2 === 3){
                            return true; //win
                        }
                    }
                }
            }
            return false;
        }
    }
}
</script>
