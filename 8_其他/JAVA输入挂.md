# JAVA输入挂

## 模版
```Java
// uwi输入挂
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.PrintWriter;
import java.math.BigInteger;
import java.util.Arrays;
import java.util.InputMismatchException;

public class Main {
	static InputStream is;
	static PrintWriter out;
	static String INPUT = "";
	
	static void solve()
	{
//		while (!eof()) {
//			
//		}
		int T = ni()
		for(int cas = 1 ; cas <= T; cas++){
			// 代码开始
			char[] s = ns().toCharArray();
			Arrays.sort(s);
			int n = s.length;
			BigInteger can = null;
			int nz = 0;
			for(int i = 0;i < n;i++){
				if(s[i] != '0')nz++;
			}
			if(nz <= 1){
				out.println("Uncertain");
			}else{
				char[] ret = new char[n];
				Arrays.fill(ret, '0');
				char sel = 0;
				int p = 0;
				for(int i = 0;i < n;i++){
					if(s[i] != '0' && sel == 0){
						sel = s[i];
					}else{
						ret[p++] = s[i];
					}
				}
				ret[0] += sel-'0';
				for(int i = 0;i < n;i++){
					if(ret[i] > '9'){
						ret[i] = (char)(ret[i]-10);
						ret[i+1]++;
					}else{
						break;
					}
				}
				boolean z = true;
				for(int i = n-1;i >= 0;i--){
					if(ret[i] == '0' && z)continue;
					z = false;
					out.print(ret[i]);
				}
				out.println();
			}
			// 代码结束
		}
	}
	
	public static void main(String[] args) throws Exception
	{
//		long S = System.currentTimeMillis(); //初始时间
//		is =System.in : new ByteArrayInputStream(INPUT.getBytes());
		is = System.in;
		out = new PrintWriter(System.out);
		
		solve();
		out.flush(); //输出
//		long G = System.currentTimeMillis(); //结束时间
//		tr(G-S+"ms");
	}
	
	private static boolean eof()
	{
		if(lenbuf == -1)return true;
		int lptr = ptrbuf;
		while(lptr < lenbuf)if(!isSpaceChar(inbuf[lptr++]))return false;
		
		try {
			is.mark(1000);
			while(true){
				int b = is.read();
				if(b == -1){
					is.reset();
					return true;
				}else if(!isSpaceChar(b)){
					is.reset();
					return false;
				}
			}
		} catch (IOException e) {
			return true;
		}
	}
	
	private static byte[] inbuf = new byte[1024];
	static int lenbuf = 0, ptrbuf = 0;
	
	private static int readByte()
	{
		if(lenbuf == -1)throw new InputMismatchException();
		if(ptrbuf >= lenbuf){
			ptrbuf = 0;
			try { lenbuf = is.read(inbuf); } catch (IOException e) { throw new InputMismatchException(); }
			if(lenbuf <= 0)return -1;
		}
		return inbuf[ptrbuf++];
	}
	
	private static boolean isSpaceChar(int c) { return !(c >= 33 && c <= 126); }
	private static int skip() { int b; while((b = readByte()) != -1 && isSpaceChar(b)); return b; }
	
	//输入double
	private static double nd() { return Double.parseDouble(ns()); }
	//输入char
	private static char nc() { return (char)skip(); }
	
	//输入一个String
	private static String ns()
	{
		int b = skip();
		StringBuilder sb = new StringBuilder();
		while(!(isSpaceChar(b))){ // when nextLine, (isSpaceChar(b) && b != ' ')
			sb.appendCodePoint(b);
			b = readByte();
		}
		return sb.toString();
	}
	
	//输入一个char数组
	private static char[] ns(int n)
	{
		char[] buf = new char[n];
		int b = skip(), p = 0;
		while(p < n && !(isSpaceChar(b))){
			buf[p++] = (char)b;
			b = readByte();
		}
		return n == p ? buf : Arrays.copyOf(buf, p);
	}
	
	//输入一个矩阵
	private static char[][] nm(int n, int m) 
	{
		char[][] map = new char[n][];
		for(int i = 0;i < n;i++)map[i] = ns(m);
		return map;
	}
	
	//输入一个输入n整数
	private static int[] na(int n)
	{
		int[] a = new int[n];
		for(int i = 0;i < n;i++)a[i] = ni();
		return a;
	}
	
	//输入一个整数
	private static int ni()
	{
		int num = 0, b;
		boolean minus = false;
		while((b = readByte()) != -1 && !((b >= '0' && b <= '9') || b == '-'));
		if(b == '-'){
			minus = true;
			b = readByte();
		}
		
		while(true){
			if(b >= '0' && b <= '9'){
				num = num * 10 + (b - '0');
			}else{
				return minus ? -num : num;
			}
			b = readByte();
		}
	}
	
	//输入一个long
	private static long nl()
	{
		long num = 0;
		int b;
		boolean minus = false;
		while((b = readByte()) != -1 && !((b >= '0' && b <= '9') || b == '-'));
		if(b == '-'){
			minus = true;
			b = readByte();
		}
		
		while(true){
			if(b >= '0' && b <= '9'){
				num = num * 10 + (b - '0');
			}else{
				return minus ? -num : num;
			}
			b = readByte();
		}
	}
	
	private static void tr(Object... o) { if(INPUT.length() != 0)System.out.println(Arrays.deepToString(o)); }
}
```