# category-tree-in-category-page

Prestashop 1.6.x by default displays only subcategories in parent category page. You can add a second level of subcategories by adding this lines of code:<br>

Installation.
Open /controllers/front/CategoryController.php , find the function "protected function assignSubcategories()" and add this code<br>
			
			$sub_sub_categories = array();
			foreach ($sub_categories as $sub_category) { 
			$sub_sub_categories[$sub_category['id_category']] = Category::getChildren($sub_category['id_category'], $this->context->language->id); 
			}
			if ($sub_sub_categories) {
            $this->context->smarty->assign(array(
                'sub_subcategories'      => $sub_sub_categories
            ));
			}
      
in this condition "if ($sub_categories = $this->category->getSubCategories($this->context->language->id)) {" before the end.<br>
Or just add a new file with a redefined function into /overrides/...<br>

Open /themes/default-bootstrap/category.tpl (might be another path if the theme is not default), and place this code:<br>

		{if isset($subcategories)}
        {if (isset($display_subcategories) && $display_subcategories eq 1) || !isset($display_subcategories) }
		<div id="subcategories">
			<ul class="subcategories">
			{foreach from=$subcategories item=subcategory name=cat}
				<li class="col-xs-12 col-sm-2">
				  <a class="subcategory-name" href="{$link->getCategoryLink($subcategory.id_category, $subcategory.link_rewrite)|escape:'html':'UTF-8'}">{$subcategory.name|escape:'html':'UTF-8'}</a>
                	<div class="subcategory-image">
						<a href="{$link->getCategoryLink($subcategory.id_category, $subcategory.link_rewrite)|escape:'html':'UTF-8'}" title="{$subcategory.name|escape:'html':'UTF-8'}" class="img">
						{if $subcategory.id_image}
							<img class="replace-2x" src="{$link->getCatImageLink($subcategory.link_rewrite, $subcategory.id_image, 'medium_default')|escape:'html':'UTF-8'}" alt="{$subcategory.name|escape:'html':'UTF-8'}" width="{$mediumSize.width}" height="{$mediumSize.height}" />
						{else}
							<img class="replace-2x" src="{$img_cat_dir}{$lang_iso}-default-medium_default.jpg" alt="{$subcategory.name|escape:'html':'UTF-8'}" width="{$mediumSize.width}" height="{$mediumSize.height}" />
						{/if}
					</a>
                   	</div>
				
				{assign var="sub_id" value=$subcategory.id_category}
			    {foreach from=$sub_subcategories.$sub_id item=sub_subcategory}
				
					<a class="sub-subcategory-name" href="{$link->getCategoryLink($sub_subcategory.id_category, $sub_subcategory.link_rewrite)|escape:'html':'UTF-8'}">{$sub_subcategory.name|escape:'html':'UTF-8'}</a>
				
			    {/foreach}
			     </li>
				  {if $smarty.foreach.cat.index == '5' || $smarty.foreach.cat.index == '11'} <div class="clearfix"></div>{/if}
			{/foreach}
			</ul> 
		</div>
        {/if}
	{/if}
	
All done!<br>

Prestashop 1.6.x по умолчанию выводит только подкатегории на страницах категорий. С помощью этих правок можно вывести подкатегории подкатегоий:<br>

Установка.
Откройте /controllers/front/CategoryController.php , найдите функцию "protected function assignSubcategories()" и добавьте этот код:<br>
			
			$sub_sub_categories = array();
			foreach ($sub_categories as $sub_category) { 
			$sub_sub_categories[$sub_category['id_category']] = Category::getChildren($sub_category['id_category'], $this->context->language->id); 
			}
			if ($sub_sub_categories) {
            $this->context->smarty->assign(array(
                'sub_subcategories'      => $sub_sub_categories
            ));
			}
      
внутри условия "if ($sub_categories = $this->category->getSubCategories($this->context->language->id)) {" перед окончанием.<br>
Или просто добавьте новый файл с переназначенной функцией в /overrides/...<br>

Откройте /themes/default-bootstrap/category.tpl (возможно, будет в другом месте, если шаблон не стандартный) и добавьте этот код:<br>

		{if isset($subcategories)}
        {if (isset($display_subcategories) && $display_subcategories eq 1) || !isset($display_subcategories) }
		<div id="subcategories">
			<ul class="subcategories">
			{foreach from=$subcategories item=subcategory name=cat}
				<li class="col-xs-12 col-sm-2">
				  <a class="subcategory-name" href="{$link->getCategoryLink($subcategory.id_category, $subcategory.link_rewrite)|escape:'html':'UTF-8'}">{$subcategory.name|escape:'html':'UTF-8'}</a>
                	<div class="subcategory-image">
						<a href="{$link->getCategoryLink($subcategory.id_category, $subcategory.link_rewrite)|escape:'html':'UTF-8'}" title="{$subcategory.name|escape:'html':'UTF-8'}" class="img">
						{if $subcategory.id_image}
							<img class="replace-2x" src="{$link->getCatImageLink($subcategory.link_rewrite, $subcategory.id_image, 'medium_default')|escape:'html':'UTF-8'}" alt="{$subcategory.name|escape:'html':'UTF-8'}" width="{$mediumSize.width}" height="{$mediumSize.height}" />
						{else}
							<img class="replace-2x" src="{$img_cat_dir}{$lang_iso}-default-medium_default.jpg" alt="{$subcategory.name|escape:'html':'UTF-8'}" width="{$mediumSize.width}" height="{$mediumSize.height}" />
						{/if}
					</a>
                   	</div>
				
				{assign var="sub_id" value=$subcategory.id_category}
			    {foreach from=$sub_subcategories.$sub_id item=sub_subcategory}
				
					<a class="sub-subcategory-name" href="{$link->getCategoryLink($sub_subcategory.id_category, $sub_subcategory.link_rewrite)|escape:'html':'UTF-8'}">{$sub_subcategory.name|escape:'html':'UTF-8'}</a>
				
			    {/foreach}
			     </li>
				  {if $smarty.foreach.cat.index == '5' || $smarty.foreach.cat.index == '11'} <div class="clearfix"></div>{/if}
			{/foreach}
			</ul> 
		</div>
        {/if}
	{/if}
	
Готово!<br>
