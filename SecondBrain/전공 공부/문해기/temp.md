```cpp
#include <iostream>
using namespace std;


struct matrix {
	long long a1, a2, a3, a4;
	matrix(long long a1, long long a2, long long a3, long long a4) {
		this->a1 = a1 % 1000000007;
		this->a2 = a2 % 1000000007;
		this->a3 = a3 % 1000000007;
		this->a4 = a4 % 1000000007;
	}
};

matrix v;

matrix square(matrix a, matrix b){
	return matrix(
		a.a1 * b.a1 + a.a2 * b.a3,
		a.a1 * b.a2 + a.a2 * b.a4,
		a.a3 * b.a1 + a.a4 * b.a3,
		a.a3 * b.a2 + a.a4 * b.a4	
	);
}

matrix findSquareOfMatrix(int squareNumber) {
	if (squareNumber == 1) {
		return v;
	}
	matrix currentMatrix = findSquareOfMatrix(squareNumber / 2);
	matrix resultMatrix = square(currentMatrix, currentMatrix);
	if (squareNumber % 2 == 0) {
		return resultMatrix;
	}
	else {
		return square(resultMatrix, v);
	}
}

int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

	int t;
	cin >> t;

	while (t--) {
		int a1, a2, a3, a4;
		cin >> a1 >> a2 >> a3 >> a4;
		int n;
		cin >> n;
		v = matrix(a1,a2,a3,a4);
		matrix result = findSquareOfMatrix(n);
		cout << result.a1 << " " << result.a2 << '\n' << result.a3 << " " << result.a4 << '\n';

	}
	return 0;
}
```