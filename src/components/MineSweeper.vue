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
    background-color: azure;
    border: 1px solid #c3c3c3;
}

.bomb{
    background-color: red;
}

.active{
    background-color: lightgray;
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
        <div class="grid">
            <div :class="{bomb: node.isBomb === true && node.active === true, active: node.isBomb === false && node.active === true}" class="grid-item" v-for="(node, index) in nodes" :key="index" v-on:click="checkNode(node)">
                <!-- {{node.x}}, {{node.y}} -->
                <div v-if="node.active === true">
                    <p v-if="node.bombCount > 0">{{node.bombCount}}</p>    
                    <p v-else-if="node.isBomb === true">{{gameBoard.bombIcon}}</p>
                </div>
            </div>
        </div>
        <canvas id="gameBoard" width="560" height="560" style="border:2px solid #d3d3d3;"></canvas>
    </div>
</template>

<script>
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
        this.gameBoard.currentDificulty = this.gameBoard.difficulties.easy;
        
        this.createNodes(
            this.gameBoard.currentDificulty.width, 
            this.gameBoard.currentDificulty.height);

        // NEW //
        var c = document.getElementById("gameBoard");
        var ctx = c.getContext("2d");

        this.drawBoard(
            this.gameBoard.currentDificulty.width,
            this.gameBoard.currentDificulty.height,
            ctx);
        
        // for(var i=0;i<8;i++){
        //     for(var j=0;j<8;j++){
        //         ctx.moveTo(0,70*j);
        //         ctx.lineTo(560,70*j);
        //         ctx.stroke();
        
        //         ctx.moveTo(70*i,0);
        //         ctx.lineTo(70*i,560);
        //         ctx.stroke();
        //         var left = 0;
        //         for(var a=0;a<8;a++) {
        //             for(var b=0; b<8;b+=2) {
        //                 var startX = b * 70;
        //                 if(a%2==0){
        //                     startX = (b+1) * 70;
        //                 } 
        //                 ctx.fillRect(startX + left,(a*70) ,70,70);
        //             }
        //         }
        //     }
        // }
    },
    methods:{
        drawBoard(width, height, ctx){

        },
        createNodes(width, height){
            for(var x = 0; x < width; x ++){
                for(var y = 0; y < height; y++){
                    const node = {
                        x: x,
                        y: y,
                        bombCount: 0,
                        flagged: false,
                        isBomb: false,
                        active: false
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
                const randomIndex = this.randomRange(0, array.length - 1);
                
                this.nodes[array[randomIndex]].isBomb = true;
                array.splice(randomIndex, 1);
            }
        },
        randomRange(min, max){
            min = Math.ceil(min);
            max = Math.floor(max);
            return Math.floor(Math.random() * (max - min + 1)) + min;
        },
        checkNode(node){
            if(node.isBomb){
                // game over
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
        flagged(){
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
