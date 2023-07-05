## hashlink 的异步回调修改

#### hashlink 不支持tcp的异步回调修改

具体使用方法可以参照 kun_net
这里修改的是hashlink 1.12 和 haxe 4.2.5 ，直接替换对应的文件，编译即可
其它版本可以按照以下的方式替换

#### c 程序修改 \libs\uv\uv.c
```c
// 新增 start =======
uv_async_t async;

void tcp_asyn_event_call_run(uv_async_t *handle)
{       
	vclosure *cl = (vclosure*)handle->data;
	hl_dyn_call(cl,NULL,0);
}


HL_PRIM void HL_NAME(tcp_asyn_event_call)(uv_loop_t *loop , vclosure *c ) {
	async.data = (void *)c;
	uv_async_send(&async);
}
// 新增 end =======

HL_PRIM uv_tcp_t *HL_NAME(tcp_init_wrap)( uv_loop_t *loop ) {
    // 新增 start =======
	uv_async_init(loop, &async, tcp_asyn_event_call_run);
    // 新增 end =======



// 新增 start =======
DEFINE_PRIM(_VOID, tcp_asyn_event_call, _LOOP _CALLB);
// 新增 end =======
```

#### haxe 程序修改 \haxe\std\hl\uv\Tcp.hx
```haxe
	public static function tcp_asyn_event_call(loop:Loop, callb:Void->Void) : Void {}
```