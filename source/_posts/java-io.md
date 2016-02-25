title: Java Input/OutputStreamReader读写文件
date: 2015-09-18 21:10:27

tags:

---




## code
<!--more-->
``` java
public class rw_file {
		public static void main(String[] args) {
			String url = "log.txt";
			try{
				BufferedReader br = new BufferedReader(
						new InputStreamReader(new FileInputStream(url),"UTF-8"));
				String data = null;
				StringBuffer str=new StringBuffer();
				while((data = br.readLine())!=null)
				{
					str.append(data+"\n");
				}
				System.out.println(str);
				br.close();
//				Test test=new Test();
//				test.ece(str.toString());
			}catch(Exception e){
				e.printStackTrace();
			}
		}
		public void ece(String str){
			try{
				BufferedWriter bw = new BufferedWriter(
						new OutputStreamWriter(new FileOutputStream(url,false),"UTF-8"));
				bw.write(str);
				bw.flush();
				bw.close();
			}catch(Exception e){
				e.printStackTrace();
			}
		}
}
```


