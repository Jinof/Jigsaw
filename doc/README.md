## API

+ ## 连接服务器
    ### nsp: /
    ### data
    ### emit
        {
            "status": 1,
            "message": "建立连接成功"
        }
        {
            "status": 0,
            "message": "建立连接失败"
        }


+ ## 获取房间列表
    ### nsp: /room ;  event: get
    ### data
        none
    ### emit event: get
        {
            "status": 0,
            "message": "房间列表获取失败"
        }
    #### 或者
        {
         "data": [
             {
                 "roomName": "小明的队伍",
                 "room_status": "游戏中"
             },
             {
                 "roomName": "小红的队伍",
                 "room_status": "等待中"
             }
         ],
            "status": 1,
            "message": "身份验证成功"
        }


+ ## 建立房间，加入房间
    ### nsp: /room ; event: join
    ### Tips: 当房间名不存在时自动创建新房间
    ### data
        {
            "username": <string>
            "roomName": <string>
            "difficult": <int>  // 难度 [1, 2, 3, 4]
        }
    ### emit event: join
    ####    建立房间
        {
            "status": 0,
            "message": "创建房间失败"
        }
        {
            "data": {
                "roomName": "小明的队伍",
                "roomId": 2412412,
                "members": [
                    {
                        "username": "小明",
                        "identity": "leader",
                        "id": 1
                    }
                ]
            }
            "status": 1,
            "message": "创建房间"
        }
    ####    加入房间
        {
            "status": 0,
            "message": "房间已满"
        }
        {
            "status": 0,
            "message": "房间正在游戏中"
        }
        {
            "status": 1,
            "message" "加入房间成功"
        }
    ##### 加入房间成功时
    #### broadcast event: broadcastRoomJoin
       {
            "data": {
                "difficult": 4,
                "roomName": "小明的队伍",
                "roomId": 12343234,
                "members": [
                    {
                        "username": "小明",
                        "identity": "leader",
                        "id": 1
                    },
                    {
                        "username": "小红",
                        "identity": "member",
                        "id": 2
                    },
                    {
                         "username": "小红",
                         "identity": "member",
                         "id": 3
                    },
                    {
                         "username": "小红",
                         "identity": "member",
                         "id": 4
                    }
                ]
            },
            "status": 1,
            "message": "小红加入房间",
       }


+ ## 离开房间
    ### nsp: /room ; event: leave
    ### data
    ### emit event: leave
        {
            "status": 1,
            "message": "离开房间成功"
        }
    #### 同时
    ### broadcast event: broadcastRoomLeave
        {
            "data": {
                "members": [
                    {
                        "username": "小明",
                        "identity": "leader",
                        "id": 1
                    }
                ],
            },
            "status": 1,
            "message": "XXX离开房间"
        }

+ ## 开始游戏
    ### nsp: / ; event: start
    ### data
    ### emit event: start
        {
            "status": 0,
            "message": "房间人数不够"
        }
    ### broadcast event: broadcastGameStart
        {
            "data": {
                "roomName": "小明的队伍",
                "picKind": 3,
                "jigsawList": [
                    [0, 2, 3, 0],
                    [0, 6, 0, 8],
                    [9, 10, 11, 12],
                    [13, 14, 15, 16]
                ],
                "difficult": 4,
                "endTime": "1568456795",
                "members": [
                    {
                        "pics": [1, 5, 7, 13],  // 可移动的图片
                        "username": "小明",
                        "identity": "leader",
                        "id": 1
                    },
                    {
                        "pics": [1, 5, 7, 13],
                        "username": "小红",
                        "identity": "member",
                        "id": 2
                    },
                    {
                        "pics": [1, 5, 7, 13],
                        "username": "小红",
                        "identity": "member",
                        "id": 3
                    },
                    {
                        "pics": [1, 5, 7, 13],
                        "username": "小红",
                        "identity": "member",
                        "id": 4
                    }
                 ],
                "status": 1,
                "message": "游戏开始",
            }
        }


+ ## 移动切片
    ### nsp: /game ; event: move
    ### data
        {
            "from": {
                "x": x,
                "y": y
            },
            "to": {
                "x": x,
                "y": y
            }
        }
    ### broadcast event: broadcastGameMove
        {
            "jigsawList": [
              [0, 2, 3, 4],
              [0, 6, 0, 8],
              [9, 10, 11, 12],
              [13, 14, 15, 16]
            ],
            "userId": 0000009001
        }


+ ## 计算成绩
    ### nsp: /game ; event: calculate
    ###
    ### data
    ### broadcast event: broadcastCalculate
        {
            score": "85"
        }


+ ## 加入游戏
    ### nsp: /game ; event: join
    ### data
        {
            "roomName": "小明的队伍"
        }
    ### emit event: join
        {
            "status": 0,
            "message": "游戏房间已满"
        }
    ### broadcast event: broadcastGameJoin
        {
            "data": [
                "pics": [4, 6, 10, 16]
            ],
            "status": 1,
            "message": "小红加入游戏"
        }


 + ## 获取排行榜
    ### nsp: / ; event: rank
    ### data
    ### emit event: rank
        {
            "data": {
                "rank": [
                    [
                        "roomName": "小明的队伍",
                        "score": 85,
                        "members": [
                            {
                                "username": "小明",
                                "userId": 0000008999,
                                "identity": "leader",
                            },
                            {
                                "username": "小李",
                                "userId": 0000009000,
                                "identity": "member",
                            },
                            {
                                "username": "小郭",
                                "userId": 0000009001,
                                "identity": "member",
                            },
                            {
                                "username": "小赵",
                                "userId": 0000009002,
                                "identity": "member",
                            }
                        ]
                    ],
                    [
                        "roomName": "小红的队伍",
                        "score": 85,
                        "members": [
                            {
                                "username": "小红",
                                "userId": 0000009003,
                                "identity": "leader",
                            },
                            {
                                "username": "小李",
                                "userId": 0000009004,
                                "identity": "member",
                            },
                            {
                                "username": "小赵",
                                "userId": 0000009005,
                                "identity": "member",
                            },
                            {
                                "username": "小郭",
                                "userId": 0000009006,
                                "identity": "member",
                            }
                        ]
                    ],
                ]
            },
            "status": 1,
            "message": "获取排行榜成功"
        }
