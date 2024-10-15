A method to represent signed integers in binary format, allowing simple arithmetic for both positive and negative numbers.

# Theory:
In two’s complement, the most significant bit (MSB) represents the sign:
- If MSB = 0, the number is positive.
- If MSB = 1, the number is negative.

The range of representable integers for $n$ bits is:

$$
-2^{n-1} \text{ to } 2^{n-1} - 1
$$

For example, an 8-bit signed integer can represent values from $-128$ to $127$.