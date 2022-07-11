# 介绍
这是一个类似电商网站的demo；



# 路由传参
```javascript

function BrandLink({ brand, children, ...props }: BrandLinkProps) {
  // 获取参数
  let [searchParams] = useSearchParams();
  let isActive = searchParams.get("brand") === brand;
  
  return (
    <Link
      to={`/?brand=${brand}`}
      {...props}
      style={{
        ...props.style,
        color: isActive ? "red" : "black",
      }}
    >
      {children}
    </Link>
  );
}

```

# 路由正则

```javascript
<Route path="/sneakers/:id" element={<SneakerView />} />


function SneakerView() {
  // 获取参数
  let { id } = useParams();
  
  if (!id) {
    return <NoMatch />;
  }
  
  let snkr = getSneakerById(id);
  
  if (!snkr) {
    return <NoMatch />;
  }
  
  let name = `${snkr.brand} ${snkr.model} ${snkr.colorway}`;
  
  return (
    <div>
      <h2>{name}</h2>
      <img
        width={400}
        height={400}
        style={{
          borderRadius: "8px",
          maxWidth: "100%",
          aspectRatio: "1 / 1",
        }}
        src={snkr.imageUrl}
        alt={name}
      />
    </div>
  );
}
```

以上两种传参方式不一样，获取参数的方式也不一样；防止混淆；  

# 响应页面
```javascript

function SneakerGrid() {
  let [searchParams] = useSearchParams();
  let brand = searchParams.get("brand");
  // 使用memo进行数据响应，并添加了相关依赖
  const sneakers = React.useMemo(() => {
    if (!brand) return SNEAKERS;
    return filterByBrand(brand);
  }, [brand]);

  return (
    <main>
      <h2>Sneakers</h2>

      <div
        style={{
          display: "grid",
          gridTemplateColumns: "repeat(auto-fill, minmax(250px, 1fr))",
          gap: "12px 24px",
        }}
      >
        {sneakers.map((snkr) => {
          let name = `${snkr.brand} ${snkr.model} ${snkr.colorway}`;
          return (
            <div key={snkr.id} style={{ position: "relative" }}>
              <img
                width={200}
                height={200}
                src={snkr.imageUrl}
                alt={name}
                style={{
                  borderRadius: "8px",
                  width: "100%",
                  height: "auto",
                  aspectRatio: "1 / 1",
                }}
              />
              <Link
                style={{ position: "absolute", inset: 0 }}
                to={`/sneakers/${snkr.id}`}
              >
                <VisuallyHidden>{name}</VisuallyHidden>
              </Link>
              <div>
                <p>{name}</p>
              </div>
            </div>
          );
        })}
      </div>
    </main>
  );
}

```