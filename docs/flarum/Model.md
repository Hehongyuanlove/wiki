# Model 模型

flarum Model 部分补充

##　模型关联

```PHP title='Goods.PHP'
    public function parent()
    {
        return $this->belongsTo(self::class, 'parent_id');
        return $this->hasMany(self::class, 'parent_id');
        return $this->hasMany(GoodsComment::class, "goods_id");
    }
```

## 模型内创建并关联事件

```PHP title='Goods.PHP'
    public static function start(User $actor, Array $goods)
    {
        $goodsMod          = new self();
        $goodsMod->price = $goods['price'];

        $goodsMod->setRelation('user', $actor);
        $goodsMod->raise(new GoodsCreate($goodsMod, $actor));

        return $goodsMod;
    }
```