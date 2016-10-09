# double的比较

## 模版
```C++
inline bool operator == (const double &a, const double &b) {
	return fabs(a - b) < eps;
}

inline bool operator != (const double &a, const double &b) {
	return fabs(a - b) > eps;
}

inline bool operator > (const double &a, const double &b) {
	return a - b > eps;
}

inline bool operator >= (const double &a, const double &b) {
	return a - b > -eps;
}

inline bool operator < (const double &a, const double &b) {
	return a - b < -eps;
}

inline bool operator <= (const double &a, const double &b) {
	return a - b < eps;
}
```