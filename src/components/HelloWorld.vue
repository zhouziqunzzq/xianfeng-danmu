<template>
    <div>
        <spirit
                v-for="sp in spirits"
                :key="sp.id"
                :id="sp.id"
                :qq="sp.qq"
                :hanashi="sp.hanashi"
                :top="sp.top"
                @nugi="nugi"
        />
    </div>
</template>

<script>
    import * as CQWebSocket from "cq-websocket";
    import Spirit from "./Spirit";

    export default {
        name: 'HelloWorld',
        components: {Spirit},
        data() {
            return {
                spirits: {},
                nowId: 0,
            };
        },
        mounted() {
            const bot = new CQWebSocket({
                host: '47.88.223.83',
                port: 6700,
            });
            bot
                .on('socket.connecting', (wsType, attempts) => {
                    console.log('尝试第 %d 次连接 _(:з」∠)_', attempts)
                })
                .on('socket.connect', (wsType, sock, attempts) => {
                    console.log('第 %d 次连接成功 ヽ(✿ﾟ▽ﾟ)ノ', attempts);
                })
                .on('socket.failed', (wsType, attempts) => {
                    console.log('第 %d 次连接失败 。･ﾟ･(つд`ﾟ)･ﾟ･', attempts)
                })
                .on('api.response', (resObj) => console.log('服务器响应: %O', resObj))
                .on('message.group', (e, context) => {
                    console.log(context);
                    if (context.group_id === 774150811) {
                        this.nowId++;
                        this.$set(this.spirits, this.nowId, {
                            id: this.nowId,
                            qq: context.user_id,
                            hanashi: context.message,
                            top: Math.random() * (document.documentElement.clientHeight - 64),
                        });
                    }
                    e.stopPropagation();
                });
            bot.connect();
        },
        props: {
            msg: String
        },
        methods: {
            nugi(id) {
                this.$delete(this.spirits, id);
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    h3 {
        margin: 40px 0 0;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        display: inline-block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
    }

</style>
