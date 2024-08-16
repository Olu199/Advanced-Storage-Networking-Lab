- **Basics**: 
  - An IP address in dotted decimal notation is composed of 4 octets (8 bits each), resulting in a total of 32 bits.
  - Example: `133.33.33.7` in dotted decimal notation.

- **Binary Conversion**:
  - Each octet in the dotted decimal notation corresponds to an 8-bit binary number.
  - Below is the binary representation of an 8-bit binary map:

| Position              | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   |
| --------------------- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Binary Position Value** | 128 | 64  | 32  | 16  | 8   | 4   | 2   | 1   |
| **Binary Value**          | 1   | 0   | 0   | 0   | 0   | 1   | 0   | 1   |

### Conversion Process from Decimal to Binary:
1. **Step 1**: Start with the leftmost binary position (128).
   - If the decimal number is greater than or equal to the binary position value, subtract the binary position value from the decimal number and place a `1` in that binary position.
   - Example: For `133`, since `133 >= 128`, subtract 128: `133 - 128 = 5`. Place a `1` in the 128 position.

2. **Step 2**: Move to the next binary position value (64).
   - If the remaining decimal number is less than the binary position value, place a `0` in that binary position.
   - Example: `5 < 64`, so place a `0` in the 64 position.

3. **Step 3**: Continue this process for each binary position.
   - For `32`, `16`, and `8`, place a `0` since `5 < 32`, `5 < 16`, and `5 < 8`.
   - When you reach the `4` position, since `5 >= 4`, subtract `4`: `5 - 4 = 1`. Place a `1` in the 4 position.
   - Continue with the `2` position, placing a `0` since `1 < 2`.
   - Finally, place a `1` in the `1` position since `1 = 1`.

4. **Final Result**: 
   - After going through all positions, the binary representation for `133` is `10000101`.
