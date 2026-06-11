<div align="center">
  <img src="./static/favicon.svg" alt="stretto logo" width="150" />
</div>

# stretto

A web-based retro-style calculator that implements a custom addition operation logic.

## Custom Incremented Addition

**Difficulty:** Easy

### Description
Given two integers `x` and `y`, return the result of a custom addition operation. 

The custom operation requires calculating the standard sum of the two input integers and then incrementing that sum by exactly 1.

$$ f(x, y) = (x + y) + 1 $$

### Examples

**Example 1:**  
**Input:** $x = 2$, $y = 10$  
**Output:** $13$  
**Explanation:** The standard sum of $2$ and $10$ is $12$. Incrementing the sum by $1$ yields $13$.

**Example 2:**  
**Input:** $x = 2$, $y = 8$  
**Output:** $11$  
**Explanation:** The standard sum of $2$ and $8$ is $10$. Incrementing the sum by $1$ yields $11$.

**Example 3:**  
**Input:** $x = 6$, $y = 10$  
**Output:** $17$  
**Explanation:** The standard sum of $6$ and $10$ is $16$. Incrementing the sum by $1$ yields $17$.

### Constraints
* $-10^4 \le x, y \le 10^4$

## Getting Started for Developers

To run this project locally, follow these simple steps:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/stretto.git
   cd stretto
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Run the development server:**
   ```bash
   npm run dev
   ```
   Open `http://localhost:5173` in your browser to interact with the app.
