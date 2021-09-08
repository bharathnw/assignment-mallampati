# assignment2-mallampati
# Sreebharath Mallampati
## Florida

Florida is affectionately known as **the sunshine state**. And for **good reason**! On average, Florida has 300 days of **sunshine** per year

******

# Road Trip to Miami
1. Open your Google Maps Application and select Miami.
2. Take a look for halting points.
3. Pick some destinations or beaches befor starting.
    1. Beaches
    2. Clubs
    3. Sports
        1. Water sports
        2. Sky diving
4. Enjoy the trip!!!

- Walt Disney World Resort
- Magic Kingdom Park
- Sanibel Island

Checkout more at my [About Me](AboutMe.md) page.

******

# Tables

Foods which contain high protein.

| Food | Location | Cost |
|------|----------|------|
|Chicken |Wallmart|$12.00 |
|Lamb|Wallmart|$18.00|
|Chick Peas|Wallmart|$6.00|
|Eggs|Any Store|$5.00|

******

# Pithy Quotes

*Julius Caesar* said:

> Friends, Romans, countrymen, lend me your ears: I come to bury Caesar, not to praise him.

*Hamlet* said:

> There are more things in heaven and earth, Horatio, than are dreamt of in your philosophy.


******

# Code Fencing

> Simple algorithms examine each pair of segments. However, if a large number of possibly intersecting segments are to be checked, this becomes increasingly inefficient since most pairs of segments are not close to one another in a typical input sequence. 

> The Shamos–Hoey applies this principle to solve the line segment intersection detection problem, as stated above, of determining whether or not a set of line segments has an intersection; the Bentley–Ottmann algorithm works by the same principle to list all intersections in logarithmic time per intersection.
 
[Click here](https://en.wikipedia.org/wiki/Multiple_line_segment_intersection) to know more about pair of intersecting segments.

```

const double EPS = 1E-9;

struct pt {
    double x, y;
};

struct seg {
    pt p, q;
    int id;

    double get_y(double x) const {
        if (abs(p.x - q.x) < EPS)
            return p.y;
        return p.y + (q.y - p.y) * (x - p.x) / (q.x - p.x);
    }
};

bool intersect1d(double l1, double r1, double l2, double r2) {
    if (l1 > r1)
        swap(l1, r1);
    if (l2 > r2)
        swap(l2, r2);
    return max(l1, l2) <= min(r1, r2) + EPS;
}

int vec(const pt& a, const pt& b, const pt& c) {
    double s = (b.x - a.x) * (c.y - a.y) - (b.y - a.y) * (c.x - a.x);
    return abs(s) < EPS ? 0 : s > 0 ? +1 : -1;
}

bool intersect(const seg& a, const seg& b)
{
    return intersect1d(a.p.x, a.q.x, b.p.x, b.q.x) &&
           intersect1d(a.p.y, a.q.y, b.p.y, b.q.y) &&
           vec(a.p, a.q, b.p) * vec(a.p, a.q, b.q) <= 0 &&
           vec(b.p, b.q, a.p) * vec(b.p, b.q, a.q) <= 0;
}

bool operator<(const seg& a, const seg& b)
{
    double x = max(min(a.p.x, a.q.x), min(b.p.x, b.q.x));
    return a.get_y(x) < b.get_y(x) - EPS;
}

struct event {
    double x;
    int tp, id;

    event() {}
    event(double x, int tp, int id) : x(x), tp(tp), id(id) {}

    bool operator<(const event& e) const {
        if (abs(x - e.x) > EPS)
            return x < e.x;
        return tp > e.tp;
    }
};

set<seg> s;
vector<set<seg>::iterator> where;

set<seg>::iterator prev(set<seg>::iterator it) {
    return it == s.begin() ? s.end() : --it;
}

set<seg>::iterator next(set<seg>::iterator it) {
    return ++it;
}

pair<int, int> solve(const vector<seg>& a) {
    int n = (int)a.size();
    vector<event> e;
    for (int i = 0; i < n; ++i) {
        e.push_back(event(min(a[i].p.x, a[i].q.x), +1, i));
        e.push_back(event(max(a[i].p.x, a[i].q.x), -1, i));
    }
    sort(e.begin(), e.end());

    s.clear();
    where.resize(a.size());
    for (size_t i = 0; i < e.size(); ++i) {
        int id = e[i].id;
        if (e[i].tp == +1) {
            set<seg>::iterator nxt = s.lower_bound(a[id]), prv = prev(nxt);
            if (nxt != s.end() && intersect(*nxt, a[id]))
                return make_pair(nxt->id, id);
            if (prv != s.end() && intersect(*prv, a[id]))
                return make_pair(prv->id, id);
            where[id] = s.insert(nxt, a[id]);
        } else {
            set<seg>::iterator nxt = next(where[id]), prv = prev(where[id]);
            if (nxt != s.end() && prv != s.end() && intersect(*nxt, *prv))
                return make_pair(prv->id, nxt->id);
            s.erase(where[id]);
        }
    }

    return make_pair(-1, -1);
}

```
For more [click here](https://cp-algorithms.com/geometry/intersecting_segments.html)
