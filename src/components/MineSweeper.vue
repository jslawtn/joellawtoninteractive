<style scoped>
#game-view{
    height: 500px;
    width: 500px;
    border: 1px solid black;
}

.grid{
    display: grid;
    max-width: fit-content;
    grid-template-columns: auto auto auto auto auto auto auto auto auto auto;
    grid-column-gap: 3px;
    grid-row-gap: 3px;
}

.grid-item{
    width: 40px;
    height: 40px;
    background-color: #a3a3a3
}

.bomb{
    background-color: red;
}

.active{
    background-color: #e2e2e2;
}

.flagged{
    background-color: lightseagreen;
}
</style>

<template>
    <div>
        <div>
            <select v-model="gameBoard.currentDificulty">
                <option :value="gameBoard.difficulties.easy">Easy</option>
                <option :value="gameBoard.difficulties.medium">Medium</option>
                <option :value="gameBoard.difficulties.hard">Hard</option>
            </select>
        </div>
        <div class="grid" ref="game-board">

            <div class="grid-item" v-for="(node, index) in nodes" v-on:click="checkNode(node)" @contextmenu="flagged(node)" 
            :class="{ 
                bomb: node.isBomb === true && node.active === true, 
                active: node.isBomb === false && node.active === true && node.flagged === false,
                flagged: node.flagged === true }" 
            :key="index" >
                <div v-if="node.active === true">
                    <p v-if="node.bombCount > 0">{{node.bombCount}}</p>    
                    <p v-else-if="node.isBomb === true">{{gameBoard.bombIcon}}</p>
                    <p v-else-if="node.flagged === true">{{gameBoard.flagIcon}}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { randomRange } from '@/helpers/random';
export default {
    name: 'mineSweeper',
    data(){
        return{
            gameBoard:{
                width: 10,
                height: 10,
                bombIcon: '💣',
                flagIcon: '🚩',
                flaggedCount: 0,
                currentDificulty: {},
                difficulties:{
                    easy:{
                        width: 10,
                        height: 10,
                        bombDistribution: 20
                    },
                    medium:{
                        width: 15,
                        height: 10,
                        bombDistribution: 25
                    },
                    hard:{
                        width: 20,
                        height: 15,
                        bombDistribution: 35
                    }
                }
            },
            nodes:[]
        }
    },
    mounted: function(){
        this.$refs['game-board'].addEventListener('contextmenu', event => event.preventDefault());
        this.gameBoard.currentDificulty = this.gameBoard.difficulties.easy;

        this.createNodes(
            this.gameBoard.currentDificulty.width, 
            this.gameBoard.currentDificulty.height);
    },
    methods:{
        createNodes(width, height){
            for(var x = 0; x < width; x ++){
                for(var y = 0; y < height; y++){
                    const node = {
                        x: x,
                        y: y,
                        bombCount: 0,
                        flagged: false,
                        isBomb: false,
                        active: false,
                        nodeConfig:{
                            'width': '40px',
                            'height': '40px',
                            'background-color':'#a3a3a3'
                        }
                    };
                    this.nodes.push(node);
                }
            }
            this.generateBombs();
        },
        generateBombs(){
            const totalNodes = this.gameBoard.width * this.gameBoard.height;
            const numberOfBombs = Math.round((this.gameBoard.currentDificulty.bombDistribution / totalNodes) * 100);
            this.gameBoard.flaggedCount = numberOfBombs;

            var array = Array.from(Array(totalNodes).keys());

            for(var i = 0; i < numberOfBombs; i++){
                const randomIndex = randomRange(0, array.length - 1);
                
                this.nodes[array[randomIndex]].isBomb = true;
                array.splice(randomIndex, 1);
            }
        },
        checkNode(node){
            if(node.isBomb){
                node.active = true;
                this.gameOver();
            }else{
                var bombCount = 0;
                for(var positionX = node.x - 1; positionX <= node.x + 1; positionX++){
                    for(var positionY = node.y - 1; positionY <= node.y + 1; positionY++){
                        var adjacentNode = this.nodes.find(x => x.x === positionX && x.y === positionY);

                        if(adjacentNode && adjacentNode.isBomb){
                            bombCount++;
                        }
                    }
                }

                node.bombCount = bombCount;
                node.active = true;
            }
        },
        flagged(node){ 
            node.flagged = !node.flagged;
            node.active = !node.active;

            this.gameBoard.flaggedCount--;
        },
        gameOver(){

        },
        restartGame(){
            this.nodes = [];
            this.createNodes(this.gameBoard.currentDificulty.width, this.gameBoard.currentDificulty.width);
        }
    },
    watch:{
        'gameBoard.currentDificulty': function(){
            this.restartGame();
        }
    }
}
</script>
