### Rush Language = Python's grammar + C++'s efficiency + Lisp's power

##### Why need it？

Reason 1: Rush support similar Python syntax without braces, and is compatible with 60% of the C++ syntax, Rush itself using standard C++ compiler to compile or self compiled.

Reason 2: Rush can be interpreted or compiled, it dominated statically typed, no GC, and support translated into C++, so can be nearly achieved C++'s efficiency.

Reason 3: It using Lisp as intermediate layer, support mixin, macro, meta-programming and a variety of dynamic characteristics, and supports call by name, call by need and call by value.

Reason 4: Rush's design goal is simple, fast, stable, fully open source, it's source code structure is much simpler than lua, but it's ability is not less than lua.

Rush overall design:

![github](https://github.com/roundsheep/rush/blob/gh-pages/doc/rush.png "github")

##### Rush coding style 1: (pseudo-code like 'Introduction to Algorithms')

		define ← =

		void insertion_sort(rstr& a):
			for j ← 1; j<a.count; j ← j+1
				key ← a[j]
				i ← j-1
				while i>=0 && a[i]>key
					a[i+1] ← a[i]
					i ← i-1
				a[i+1] ← key

##### Rush coding style 2: (like Python without braces)

		bool next_permutation<T>(rbuf<T>& v):
			if v.count<=1  
				return false  
			next=v.count-1  
			for  
				temp=next  
				next--  
				if v[next]<v[temp]  
					mid=v.count-1  
					for !(v[next]<v[mid])  
						mid--  
					swap<T>(v[next],v[mid])  
					reverse<T>(v,temp)  
					return true  
				if next==0  
					reverse<T>(v,0)  
					return false

##### Rush coding style 3: (Lisp style, S expressions separated by commas)

		void main():
			int a
			int b
			[int,=,[a,1]]
			[int,=,[b,2]]
			[rf,print,[[int,+,[a,b]]]]
			
##### Rush coding style 4: (This is the standard C++ syntax, this section of code can be compiled with VC++, G++ or Rush)

		static rbool inherit_proc(tsh& sh,tclass& tci,int level=0)
		{
			if(level++>c_rs_deep)
				return false;
			if(tci.vfather.empty())
				return true;
			rbuf<tword> v;
			for(int i=0;i<tci.vfather.count();i++)
			{
				rstr cname=tci.vfather[i].vword.get(0).val;
				tclass* ptci=zfind::class_search(sh,cname);
				if(ptci==null)
				{
					ptci=zfind::classtl_search(sh,cname);
					if(ptci==null)
						return false;
				}
				if(!inherit_proc(sh,*ptci,level))
					return false;
				v+=ptci->vword;
			}
			v+=tci.vword;
			tci.vword=v;
			return true;
		}

<br/>
##### Rush's benchmark:

(Intel i5 3.3GHZ, using command 'xxx -nasm ..\src\rush.cxx' self compiled , scan code about 25000 lines, get a ASM file with 7M , it shows the score 1/3 of Rush PK C++. Because of Rush itself using C++11's move and short string optimization, but Rush itself not support the optimization, so I Can believe the really Rush's benchmark can be achieved C++'s 1/1.5, this is close to C++. It also shows Rush's compile speed is very fast, if don't use nasm, it compiled itself requires only 5 seconds)

![github](https://github.com/roundsheep/rush/blob/gh-pages/doc/benchmark.png "github")

<br/>
Rush supports multiple operation modes, as follows:

##### JIT:

1. cd to the bin directory
2. Command line typing: rush -jit ..\src\example\test\1.rs

##### Interpret:

1. cd to the bin directory
2. Command line typing: rush ..\src\example\test\1.rs

##### Compile and run (using NASM as backend):

1. cd to the bin directory
2. Command line typing: rnasm ..\src\example\test\1.rs

##### Compile and run (translate to C++ and using G++ as backend):

1. cd to the bin directory
2. Command line typing: gpp ..\src\example\test\1.rs

##### Compile and run (translate to ASM, then translate to C++, in alpha):

1. cd to the bin directory
2. Command line typing: rcpp ..\src\example\test\53.rs

##### Binary translation(EXE to C to Rush to JS, only support simple code now, in alpha):

1. cd to the bin directory
2. using G++ compile standard C++ code, command line typing: mingw_build ..\ext\ida\test.cpp
3. At this time, get a stanadard PE file: ext\ida\test.cpp.exe
4. Download and unzip: IDA pro 6.6（161M） http://pan.baidu.com/s/1kTs8K1h
5. cd to IDA home directory
6. Command line typing: idaq -A ..\rush\ext\ida\test.cpp.exe (depending on the unzip path)
7. Press Ctrl+F5, Wait a few minutes, get file: ext\ida\test.cpp.c
8. cd to bin, Command line typing: rush -jit ..\ext\ida\trans.rs ..\ext\ida\test.cpp.c
9. At this time, get file: ext\ida\test.rs
10. Command line typing: rush -js ..\ext\ida\test.rs
11. Double-click ext\ida\test.html

##### HTML5 online interpretation run (currently only support chrome, emscripten function pointer conversion support for seemingly a problem):

1. Click http://roundsheep.github.io/rush/
2. Click the 'run' button and wait a few seconds will show operating results

##### HTML5 run (translate to JS, not emscripten, currently only support chrome, in alpha)

1. cd to the bin directory
2. Command line typing: rush -js ..\src\example\test\53.rs
3. Double-click ..\src\example\test\53.html

##### Mac OS or Ubuntu interpretation runs (in alpha):

1. Make sure the clang (3.5 or higher) or g++ (4.8 or higher)
2. cd to the bin directory
3. Command line typing: g++ ../src/rush.cxx -o rush -w -m32 -std=c++11
4. Command line typing: ./rush ../src/example/test/50.rs

##### IOS:

1. cd to the bin directory
2. Command line typing: rush -gpp ..\src\example\test\1.rs
3. The resulting src\example\test\1.cpp and ext\mingw\gpp.h two files into xcode
4. Modify the main function as required, comment out the header files 'windows.h'

##### Android: (green integrated package, independent of other environments)

1. Ensure that the compiled environment is windows with 64 bit
2. Download a key android toolkit and unzip to a path without space or Chinese (1.1G) http://pan.baidu.com/s/1c0oc3Ws
3. Click create_proj.bat
4. Enter the project name such as 'test', waiting for the end of the command window
5. Click proj\test\build_cpp.bat
6. Wait a few minutes, command window appears "Press any key to continue."
7. Press 'Enter' and wait for the end of the command window
8. Get proj\test\proj.android\bin\xxx.apk after successfully
9. If need, use 'rush -gpp' command to translate Rush to CPP and include it into the proj\test\Classes\HelloWorldScene.cpp

##### X64:

1. cd to the bin directory
2. Command line typing: rush -gpp64 ..\src\example\64bit_test.rs
3. Import the resulting file src\example\test\64bit_test.cpp with Visual Studio (or using 64 of G++)
4. Select x64, Press F7

##### Third party IDE edit code:

1. Run ext\ide\SciTE.exe
2. Click File -> Open
3. Select the ..\src\example\test\1.rs, click 'Open'
4. Press F5 to run the program (or F7 generate EXE)

<br/>
##### Rush re-compiled to JS:

1. Be sure to install the emscripten
2. Open 'Emscripten Command Prompt'
3. cd to Rush home directory
4. Command line typing: em++ -O3 src\rush.cxx -o rush.html -w --preload-file src -s TOTAL_MEMORY=156777216 -s EXPORTED_FUNCTIONS="['_ js_main']"

##### Rush re-compiled from source code:

1. Be sure to install the VS2012 update4 or VS2013
2. Open src\proj\rush.sln
3. Select 'Release' mode (not support 'Debug' because of JIT)
4. Press F7, will generate bin\rush.exe after successfully

##### Self compiled (using NASM as backend):

1. Double-click self_test_nasm.bat
2. Wait a few minutes will generate bin\rush_nasm.exe (Rush actually complete self compiled requires only 5 seconds, the bottleneck in NASM, reportedly Chez Scheme self compiled is 5 seconds)
3. Note that only NASM mode and GPP mode are available after self compiled

##### Self compiled (using G++ as backend):

1. Double-click self_test_gpp.bat
2. Wait a few minutes will be generated bin\rush_gpp.exe
3. Note that only NASM mode and GPP mode are available after self compiled

##### Debug:

1. cd to the bin directory
2. Command line typing: rush -gpp ..\src\example\test\1.rs
3. Using gdb or Visual Studio to debugging the src\example\test\1.cpp

##### Auto test example:

1. Double-click the bin\example_test.bat

<br/>

QQ group: 34269848

E-mail: 287848066@qq.com
