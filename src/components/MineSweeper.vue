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
</style>

<template>
    <div>
        <div class="grid">
            <div :class="{bomb: node.isBomb === true}" class="grid-item" v-for="(node, index) in nodes" :key="index" v-on:click="checkNode(node)">
                {{node.x}}, {{node.y}}
                <p v-if="node.active === true">{{node.bombCount}}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'mineSweeper',
    data(){
        return{
            gameView:{},
            gameBoard:{
                width: 10,
                height: 10
            },
            nodes:[]
        }
    },
    mounted: function(){
        this.createNodes(this.gameBoard.width, this.gameBoard.height);
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
                        active: false
                    };

                    this.nodes.push(node);
                }
            }

            this.generateBombs();
        },
        generateBombs(){
            const totalNodes = this.gameBoard.width * this.gameBoard.height;
            const numberOfBombs = Math.round((25 / totalNodes) * 100);

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
        }
    }
}
</script>
