# hello-world
这是我的第一个程序
每日总结
今天学习了TCP网络通信协议

/////////////////////////服务端//////////////////////

public class Server{
	 private Integer port;//私有的属性port作为端口号
   
	    private ServerSocket server;//私有属性ServerSocket作为服务端Socket提供服务
	
    	 public Server(Integer port) {//有参构造
		   this.port=port;
		   try {
		  	   server=new ServerSocket();
	        	} catch (IOException e) {
			       e.printStackTrace();
		        }
	    }

//作为一条线程新建一个私有内部类ClientHandler实现Runnable

	private class ClientHandler implements Runnable{
      
			private Socket socket;//①私有属性：Socket socket
			public ClientHandler(Socket socket) {// ②有参构造
				this.socket=socket;
			}
			
			//
		@Override
		public void run() {//③重写run方法，在run方法中写一个此socket的输入流进行包装并无线循环输出接收的内容
			while(true) {
				try {
					//输入流
					InputStream is=socket.getInputStream();
					BufferedReader br=new BufferedReader(
							new InputStreamReader(is,"UTF-8"));
					String str=null;
					while((str=br.readLine())!=null) {
						System.out.println(str);
					}
				} catch (IOException e) {
					e.printStackTrace();
				}
		}
		}
		
	}

	//新建一个服务器启动方法start
	public void start() {
     
	 ServerSocket ss = null;
	try {
		 ss= new ServerSocket(port);
	} catch (IOException e1) {
		// TODO Auto-generated catch block
		e1.printStackTrace();
	}

		while(true) {
			try {
				System.out.println("等待客户端连接...");
			Socket s=ss.accept();// (1)在start方法中使用循环监听客户端连接
			System.out.println("客户端连接成功...");
				//(2)把连接返回的socket作为参数构造一个ClientHandler任务，并把它交给一个线程启动
				new Thread(new ClientHandler(s)).start();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
		
	}
  
    public static void main(String[] args){
          // 在main方法中测试Server服务
                Server d = new Server(9999); 
		                  d.start();
                    }
 }

//////////////////////////客户端////////////////////////

  public class SocketTest {

      public static void main(String[] args) {
          	Scanner sc=new Scanner(System.in);
	                try {
	                	Socket st=new Socket("192.168.43.81",9999);
	                  	OutputStream os=st.getOutputStream();//输出流
		                PrintWriter pw=new PrintWriter(
				            new OutputStreamWriter(os,"UTF-8"),true);//打印流
		          while(true) {//无线输出
			      String str=sc.next();//键盘输入
		    	pw.println(str);//
	  	}
	  } catch (IOException e) {
		    e.printStackTrace();
		    sc.close();
	    }
    }
  }

