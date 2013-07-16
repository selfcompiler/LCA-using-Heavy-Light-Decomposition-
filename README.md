LCA-using-Heavy-Light-Decomposition-
====================================
LCA using Heavy Light Decomposition 

    #include <cstdio>
    #include <vector>
    #include <algorithm>
 
    using namespace std;
 
    const int MAXM = 16;
    const int MAXN = 1 << MAXM;
 
      // Heavy-Light Decomposition
    struct TreeDecomposition {
    vector<int> e[MAXN], c[MAXN];
    int s[MAXN]; // subtree size
    int p[MAXN]; // parent id
    int r[MAXN]; // chain root id
    int t[MAXN];// timestamp, index used in segtree
    int lvl[MAXN];
    int ts;
 
    void dfs_(int v, int f) {
    p[v] = f;
    s[v] = 1;
    for (int i = 0; i < (int)e[v].size(); ++i) {
      int w = e[v][i];
      if (w != f) {
        dfs_(w, v);
        s[v] += s[w];
      }
      }
    }
 
    void decomp_(int v, int f, int k) {
    t[v] = ts++;
    c[k].push_back(v);
    r[v] = k;
 
    int x = 0, y = -1;
    for (int i = 0; i < (int)e[v].size(); ++i) {
      int w = e[v][i];
      if (w != f) {
        if (s[w] > x) {
          x = s[w];
          y = w;
        }
      }
    }
    if (y != -1) {
      decomp_(y, v, k);
    }
 
    for (int i = 0; i < (int)e[v].size(); ++i) {
      int w = e[v][i];
      if (w != f && w != y) {
        decomp_(w, v, w);
      }
      }
    }
 
    void init(int n) {
    for (int i = 0; i < n; ++i) {
      e[i].clear();
     }
    }
 
      void add(int a, int b) {
      e[a].push_back(b);
      e[b].push_back(a);
    }
 
      void build() { // !!
      ts = 0;
      dfs_(0, 0);
      decomp_(0, 0, 0);
      }
    void print()
    {
      for(int i=0;i<15;i++)
      printf("%d ",s[i]);
      printf("\n");
      
      for(int i=0;i<15;i++)
      printf("%d ",p[i]);
      printf("\n");
      
      for(int i=0;i<15;i++)
      printf("%d ",r[i]);
      printf("\n");
      
    
      for(int i=0;i<15;i++)
      printf("%d ",s[i]);
      printf("\n");
      
       for(int i=0;i<15;i++)
       printf("%d ",lvl[i]);
      printf("\n");
      
      
      }
      void getlvl(int n,int l)
      {
      lvl[n]=l;
      //printf("assigne= %d %d\n",n,l);
      for(int i=0;i<(int)e[n].size();i++)
      {
          if(e[n][i]!=p[n])
          {
             // printf("lvl = %d\n",e[n][i]);
              getlvl(e[n][i],l+1);
          }
      }
      }
      int lca(int a,int b)
      {
      while(r[a]!=r[b])
      {
          printf("%d %d %d %d\n",a,b,r[a],r[b]);
          if(lvl[a]<lvl[b]) 
         { b=p[r[b]];}
          else if(lvl[a]>lvl[b])
          {a=p[r[a]];}
          else if(lvl[a]==lvl[b])
          {
              if(r[a]>r[b])
              {
                  a=p[r[a]];
              }
              else
              {
                  b=p[r[b]];
              }
          }
          
            
      }
      printf("%d %d %d %d\n",a,b,r[a],r[b]);
      if(lvl[a]<lvl[b])
      return a;
      else
      return b;
      }
      } ;
      int main()
    {
    int a,b,no;
    TreeDecomposition hld;
    scanf("%d",&no);
    hld.init(no);
    for(int i=0;i<no;i++)
    {
        scanf("%d%d",&a,&b);
        hld.add(a,b);
    }
    hld.build();
    scanf("%d%d",&a,&b);
    hld.getlvl(0,0);
    hld.print();
    printf("%d ",hld.lca(a,b));
    }
