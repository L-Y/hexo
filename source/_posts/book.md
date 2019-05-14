title: mysql 大数据查询分页
date: 2019-05-14 17:10:41
tags: laravel
---

## mysql 数据量过大, 导致分页查询过慢解决办法


```mysql
DB::select("select a.title,a.name from 
(SELECT `id` FROM `article`  WHERE `status` = ? AND `updated_at` > ? LIMIT ? OFFSET ? ) c 
LEFT JOIN article a ON a.id = c.id 
LEFT JOIN `detail` AS `ab` ON `a`.`id` = `ab`.`article_id` ",[1,'2018-12-01',$limit,$offset]
```

## 自定义函数

```php
    /**
     * updateBigDataBySql 用SQL拼接批量更新
     *
     * @param array $multipleData 更新数据
     *
     * @return bool|int
     */
    public function updateBigDataBySql($multipleData = []){
        try {
            if (empty($multipleData)) {
                throw new \Exception("数据不能为空");
            }
            $tableName = $this->model->getTable(); 
            $firstRow  = current($multipleData);

            $updateColumn = array_keys($firstRow);
            // 默认以id为条件更新，如果没有ID则以第一个字段为条件
            $referenceColumn = isset($firstRow['id']) ? 'id' : current($updateColumn);
            unset($updateColumn[0]);
            // 拼接sql语句
            $updateSql = "UPDATE " . $tableName . " SET ";
            $sets      = [];
            $bindings  = [];
            foreach ($updateColumn as $uColumn) {
                $setSql = "`" . $uColumn . "` = CASE ";
                foreach ($multipleData as $data) {
                    $setSql .= "WHEN `" . $referenceColumn . "` = ? THEN ? ";
                    $bindings[] = $data[$referenceColumn];
                    $bindings[] = $data[$uColumn];
                }
                $setSql .= "ELSE `" . $uColumn . "` END ";
                $sets[] = $setSql;
            }

            $updateSql .= implode(', ', $sets);
            $whereIn   = collect($multipleData)->pluck($referenceColumn)->values()->all();
            $bindings  = array_merge($bindings, $whereIn);
            $whereIn   = rtrim(str_repeat('?,', count($whereIn)), ',');
            $updateSql = rtrim($updateSql, ", ") . " WHERE `" . $referenceColumn . "` IN (" . $whereIn . ")";
            // 传入预处理sql语句和对应绑定数据
            return DB::update($updateSql, $bindings);
        } catch (\Exception $e) {
            return false;
        }
    }
```