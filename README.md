Download Link: https://assignmentchef.com/product/solved-comp273-assignment-4-word-count
<br>
<h1></h1>

Using the template <em>wordcount.asm </em>as a starting point, you are to write a MIPS program that counts the number of occurrences of a word in a text block entered using the keyboard. The user enters a text block (no more than 600 characters) in the console and then asks for a word to search for. Note that a word means any continuous set of characters before the next space. The program should search the text block and display the number of occurrences of the word entered. The program should print the text and the word entered by the keyboard. The end of the text block is reached when the user presses the &lt;Enter&gt; key. After the program finishes it should provide an option of redoing the task or exiting the program.

You should implement the program using memory-mapped I/O, i.e., you are not allowed to use the syscall. A sample output would be like this:

Word count

Enter the text segment:

Roses are red, the sky is blue and firetrucks are red.

Enter the search word:

red

The word ’red’ occurred 2 time(s).

press ’e’ to enter another segment of text or ’q’ to quit.

Your code should handle cases where the search word is not in the text block. For example:

Word count

Enter the text segment:

I love assembly language. Language is my thing!

Enter the search word:

computer

The word ’computer’ occurred 0 time(s). press ’e’ to enter another segment of text or ’q’ to quit.

Note that for this assignment you need to use the “Keyboard and Display MMIO Simulator” in MARS by connecting it to MIPS. This can be found under Tools-&gt;Keyboard and Display MMIO Simulator. Click Connect to MIPS. In the main MARS window, after assembling your code, select the 0xffff0000 (MMIO) portion of the data segment. This is the memory mapped IO area. Click Reset in the MMIO simulator window.

<h1>2           QuickSort using Memory-Mapped I/O</h1>

You are required to implement the quicksort algorithm using the MIPS assembly language by completing the <em>quicksort.asm </em>template file provided to you. Your program must use memory-mapped I/O for both the inputs and outputs.

<ol>

 <li>As inputs, consider only numbers that have up to two digits.</li>

 <li>The numbers will be separated by a single blank space.</li>

 <li>Your program should work correctly on any valid inputs. Also, it should ignore any unknown keys (see the table below for known keys). You may also assume that the user will not enter three consecutive digits.</li>

 <li>Your program must echo (print) the digits entered by the user.</li>

 <li>Note that entries such as ’c’, ’s’ and ’q’ are not to be echoed on the screen. 6. You may assume that the program will not be tested on negative numbers.</li>

 <li>You may also assume a fixed size array (containing at-least 10 elements).</li>

 <li>In the event that the user presses s with less than 10 numbers having being entered, the results displayed should be only for the numbers entered.</li>

 <li>If the array has not been cleared (i.e ’c’ has not been pressed), the elements of the array entered previously should be combined with the new entries, before the sorting algorithm is applied.</li>

 <li>You may assume that no attempt will be made to check for the overflow of the array,</li>

</ol>

i.e., that the count of numbers entered will never exceed the size of the array.

<table width="0">

 <tbody>

  <tr>

   <td width="63">key</td>

   <td width="186">Meaning</td>

   <td width="47">Echo</td>

  </tr>

  <tr>

   <td width="63">0-9</td>

   <td width="186">The digits 0 to 9</td>

   <td width="47">yes</td>

  </tr>

  <tr>

   <td width="63">SPACE</td>

   <td width="186">The blank space</td>

   <td width="47">yes</td>

  </tr>

  <tr>

   <td width="63">c</td>

   <td width="186">Clear/reinitialize the array</td>

   <td width="47">no</td>

  </tr>

  <tr>

   <td width="63">s</td>

   <td width="186">Display the sorted array</td>

   <td width="47">no</td>

  </tr>

  <tr>

   <td width="63">q</td>

   <td width="186">Quit the program</td>

   <td width="47">no</td>

  </tr>

 </tbody>

</table>

<h1>Guidelines</h1>

In order to give you an idea how the program should work, here is a sample output

Welcome to QuickSort

12 3 4 67 89 &lt;s&gt;

The sorted array is: 3 4 12 67 89

99 78 &lt;c&gt;

The array is re-initialized

20 13 56 99 &lt;s&gt;

The sorted array is: 13 20 56 99

3 40 99 78 0 10 &lt;s&gt;

The sorted array is: 0 3 10 13 20 40 56 78 99 99

&lt;q&gt;

&lt;program ends&gt;

The commands in brackets &lt;s,c,q&gt; are not echoed on-screen

<ol>

 <li>In order to understand the working of the program, a schematic diagram is provided with this assignment. This provides one way to approach the problem. This is by NO MEANS the only way to do this assignment. Please feel free to implement your own design.</li>

 <li>There are some subtle points that have to be carefully worked out. One important point is how to deal with sorting an incomplete array of elements. One way is to maintain a variable which will contain the the maximum index value of the filled array. Initially it will be set to 0, but it will be updated whenever new numbers are entered. If the user presses c, the index value will be reset to 0.</li>

 <li>Take special care when dealing with a two digit number. For example if the user inputs 23, you will receive the input in the following order : 2 3. In order to generate the numerical value of 23, you will have to multiply the 2 by 10 and then add 3.</li>

</ol>

Figure 1: Flowchart for quicksort program flow.

QuickSort

QuickSort is an in-place sort algorithm that uses the divide and conquer paradigm. It has two phases: the partition phase and the sort phase. It picks an element from the array (the pivot), partitions the remaining elements into those greater than and less than this pivot, and recursively sorts the partitions. In the most general case, we don’t know anything about the items to be sorted, so any choice of pivot element works. (The first element is a convenient choice.) You can choose to implement any strategy for the choice of pivot.

There are some excellent websites with java applets showing quicksort in action. One such website is:

https://visualgo.net/bn/sorting

The C source code for quicksort is provided below.

<table width="0">

 <tbody>

  <tr>

   <td width="237">#include &lt;stdio .h&gt;#define LISTSIZE 5 int l i s t [ LISTSIZE ] ; void quicksort ( int [] , int , int</td>

   <td width="334">) ;</td>

  </tr>

  <tr>

   <td width="237">int           partition ( int           [] ,      int ,         int</td>

   <td width="334">) ;</td>

  </tr>

  <tr>

   <td width="237">void swap( int               [] , int , int       ) ;int main( int argc , char ∗argv [ ] ) int val ; int i ;// read in the data</td>

   <td width="334">{</td>

  </tr>

 </tbody>

</table>


