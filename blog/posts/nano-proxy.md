#一个不到30行的可运行python http proxy
>2014/9/1

直接上代码，支持多线程，http协议处理方面还需要细化


    import socket,threading

    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM) 

    sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

    sock.bind(('0.0.0.0', 8001))  

    sock.listen(5)

    try:

        while True:  

            local,address = sock.accept() 

            def process(local):

                try:  

                    local.settimeout(30)  

                    buf = local.recv(1024*8)  

                    p=buf.find("Host:")

                    p1=buf.find("\r\n",p)

                    host= buf[p+6:p1].strip()

                    print host

                    remote_sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  

                    remote_sock.connect((host, 80))

                    remote_sock.send(buf.replace('HTTP/1.1','HTTP/1.0'))#avoid keep-live

                    while True:

                        buf1=remote_sock.recv(1024)

                        if not buf1:break

                        local.send(buf1)

                    remote_sock.close()

                    local.close()

                except:print 'error remote'

            threading.Thread(target = process,args=(local,)).start()

    except:print 'error server'

    finally:sock.close()
