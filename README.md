## hashlink ���첽�ص��޸�

#### hashlink ��֧��tcp���첽�ص��޸�

����ʹ�÷������Բ��� kun_net
�����޸ĵ���hashlink 1.12 �� haxe 4.2.5 ��ֱ���滻��Ӧ���ļ������뼴��
�����汾���԰������µķ�ʽ�滻

#### c �����޸� \libs\uv\uv.c
```c
// ���� start =======
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
// ���� end =======

HL_PRIM uv_tcp_t *HL_NAME(tcp_init_wrap)( uv_loop_t *loop ) {
    // ���� start =======
	uv_async_init(loop, &async, tcp_asyn_event_call_run);
    // ���� end =======



// ���� start =======
DEFINE_PRIM(_VOID, tcp_asyn_event_call, _LOOP _CALLB);
// ���� end =======
```

#### haxe �����޸� \haxe\std\hl\uv\Tcp.hx
```haxe
	public static function tcp_asyn_event_call(loop:Loop, callb:Void->Void) : Void {}
```