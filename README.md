# 前端常考代码题

## 排序

### 冒泡排序

```c++
void bubbleSort(int a[], int len) {
	for (int i = 0; i<len; ++i) {
		for (int j = 0; j<len - i - 1; ++j) {
			if (a[j]>a[j + 1]) {
				swap(a[j], a[j + 1]);
			}
		}
	}
}

int main() {
	int num;
	cin >> num;
	int *a = new int[num];
	for (int i = 0; i<num; ++i) {
		cin >> a[i];
	}
	bubbleSort(a, num);
	for (int j = 0; j<num; ++j) {
		cout << a[j];
	}
	system("pause");
	return 0;
}
```

