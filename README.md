# correctSub

package sub;

import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintStream;
import java.util.ArrayList;
import java.util.Scanner;

public class Sub {

	public static void main(String[] args) throws IOException {

		Scanner scan = new Scanner(System.in);
		System.out.println("Input correction in second = ");
		int correctSec = scan.nextInt();
		ArrayList<String> al = new ArrayList<String>();
		String file = null;

		FileInputStream fis = new FileInputStream("C:\\Users\\viki\\Desktop\\buky\\Wind.txt");
		PrintStream correctSub = new PrintStream("C:\\Users\\viki\\Desktop\\buky\\test.txt");
		BufferedReader br = new BufferedReader(new InputStreamReader(fis));
		while ((file = br.readLine()) != null) {
			String arr[] = file.split(" ");
			if (arr.length == 3) {
				for (int i = 0; i < 3; i++) {
					al.add(arr[i]);
				}
				if (al.get(0).length() == 12 && al.get(0).length() == al.get(2).length() && al.get(1).length() == 3) {
					String timeBegin = changeTime(al.get(0), correctSec);
					String timeStop = changeTime(al.get(2), correctSec);
					correctSub.println(timeBegin + " --> " + timeStop);
				} else {
					correctSub.println(file);
				}
				al.clear();
			} else {
				correctSub.println(file);
			}
		}
		scan.close();
		br.close();
		correctSub.close();

	}

	private static String changeTime(String text, int correctSec) {

		StringBuilder sb = new StringBuilder(text);
		int sec = Integer.parseInt(text.substring(6, 8));
		int min = Integer.parseInt(text.substring(3, 5));
		int hours = Integer.parseInt(text.substring(0, 2));

		sec = sec + correctSec;
		int minPlus = sec / 60;
		sec = sec % 60;
		// for positive number (+)
		if (minPlus >= 0 && sec >= 0) {
			min = min + minPlus;
			if (min >= 60) {
				min = min - 60;
				hours = hours + 1;
			}
		}
		// for negative number (-)
		if (minPlus < -1) {
			sec = sec + 60;
			if (sec == 60) {
				sec = 0;
			}
			min = (min - 1) + minPlus;
			if (min < 0) {
				min = min + 60;
				hours = hours - 1;
			}
		}
		if (sec <= 0 && minPlus >= -1) {
			sec = sec + 60;
			if (sec == 60) {
				sec = 0;
			}
			min = (min - 1) + minPlus;
			if (min < 0) {
				min = min + 60;
				hours = hours - 1;
			}
		}
		sb.replace(6, 8, makeString(sec));
		sb.replace(3, 5, makeString(min));
		sb.replace(0, 2, makeString(hours));

		return sb.toString();

	}

	private static String makeString(int time) {
		String stringTime = Integer.toString(time);
		if (stringTime.length() < 2) {
			stringTime = "0" + stringTime;
		}
		return stringTime;
	}
}
