# 使用简述
```bash
推流
ffmpeg -re -i test.mp4 -vcodec h264 -acodec aac -f flv rtmp://127.0.0.1/live/test

拉流
ffplay -i rtmp://127.0.0.1:1935/live/test
```

# 构建顺序(运行逻辑)

```cpp
1.  EventLoop
①TaskScheduler  ->
                Pipe->TcpSocket
                               ->Logger->TimeStamp
                               ->SocketUtil
                -> 
                Time
                
                ->
                Channel
                    
                ->
                RingBuffer
                    
②EpollTaskScheduler


③SelectTaskScheduler


2.  RtmpServer

①TcpServer -> 
            TcpConnection
                        ->BufferReader
                        ->BufferWriter
                        ->SocketUtil
            
            ->
            Acceptor
            
②Rtmp

③RtmpConnection ->
                 RtmpSink -> amf
                 
                 ->
                 RtmpPublisher 
                 
                 ->
                 RtmpClient 
                 
                 ->
                 RtmpSession 
                 
                 ->
                 RtmpHandshake 
                 
                 ->
                 RtmpChunk 
                 
                 ->
                 RtmpMessage 
```