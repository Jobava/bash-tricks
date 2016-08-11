## get wifi IP address

```bash
sudo ifconfig | sed -n -e '/wlp3s0/,$p' | grep inet | head -n 1 | sed 's/^[ \t]*\(.*\)/\1/' | cut -d' ' -f 2
```

* wlp3s0 is the name of the network interface
* n disables default behavior of sed of printing each line after executing its script on it
* -e indicated a script to sed
* $ means scan to the end of the file
* p is the print command in sed
* sed 's/^[ \t]*\(.*\)/\1/' we capture all except the leading whitespaces
* \1 is the first capture group
* escaped parens \( \)  signify a capture group in sed
* cut -d' ' -f 2  cuts the text by a whitespace delimiter and takes the second column

To get the name of the network interface, first do this:

```bash
sudo lshw -class network | sed -n -e '/[Ww]ireless/,$p' | grep -i "logical name:" | head -n 1 | sed 's/^[ \t]*\(.*\)/\1/' | cut -d' ' -f 3
```

Together, these combined make

```bash
my_wlan=$( sudo lshw -class network | sed -n -e '/[Ww]ireless/,$p' | grep -i "logical name:" | head -n 1 | sed 's/^[ \t]*\(.*\)/\1/' | cut -d' ' -f 3 ); sudo ifconfig | sed -n -e "/$my_wlan/,\$p" | grep inet | head -n 1 | sed 's/^[ \t]*\(.*\)/\1/' | cut -d' ' -f 2
```

notice a little change made in this place: ```sed -n -e "/$my_wlan/,\$p"``` from single quotes to double-quotes, which requires the escaping of the dollar sign

