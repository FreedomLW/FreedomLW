#define _USE_MATH_DEFINES

#include <iostream>
#include <math.h>
#include <iomanip>

const long double eps = 10e-9;
long double true_ans = M_PI / 3.;

long double f(long double x) {
    x *= x;
    return long double(1./(1. + x));
}

long double I(int n) {
    long double a = 0.;
    long double b = sqrt(3.) / (n);
    long double delta = b;
    long double m = b / 2.;
    long double ans = 0.;
    while (b <= sqrt(3) + eps) {
        ans += (delta / 6.) * (f(a) + 4.*f(m) + f(b));
        a += delta;
        b += delta;
        m += delta;
    }
    return ans;
}

long double err(long double ans) {
    return std::abs(ans - true_ans);
}

long double s(long double err, int n) {
    return log(err) / (log(sqrt(3)) - log(n));
}

#include <vector>

int main() {
    int i = 0;
    std::vector<long double> I_;
    std::vector<long double> errors;
    std::vector<long double> s_;
    for (i = 8; i <= pow(2, 15); i *= 2) {
        I_.push_back(I(i));
        errors.push_back(err(I_[I_.size() - 1]));
        s_.push_back(s(errors[errors.size() - 1], i));
    }
    std::cout << "n\t";
    for (i = 8; i <= pow(2, 15) + eps; i *= 2) {
        std::cout << static_cast<int>(i) << "\t\t";
    }
    std::cout << "\nerr_n\t";
    for (size_t j = 0; j < errors.size(); ++j) {
        std::cout << errors[j] << "\t";
    }
    std::cout << "\ns_n\t";
    for (size_t j = 0; j < s_.size(); ++j) {
        std::cout << s_[j] << "\t\t";
    }
}
