---
layout: post
title: spring ioc 双向引用
description: spring ioc 双向引用
category: blog
---

public abstract class AbstractBeanFactory implements BeanFactory {

	@Override
	public Object getBean(String name) throws Exception {
		BeanDefinition beanDefinition = beanDefinitionMap.get(name);
		if (beanDefinition == null) {
			throw new IllegalArgumentException("No bean named " + name + " is defined");
		}
		Object bean = beanDefinition.getBean();
		if (bean == null) {
			bean = doCreateBean(beanDefinition);
		}
		return bean;
	}
	}


public class AutowireCapableBeanFactory extends AbstractBeanFactory {

	@Override
	protected Object doCreateBean(BeanDefinition beanDefinition) throws Exception {
		Object bean = createBeanInstance(beanDefinition);
        beanDefinition.setBean(bean);
		applyPropertyValues(bean, beanDefinition);
		return bean;
	}

	protected Object createBeanInstance(BeanDefinition beanDefinition) throws Exception {
		return beanDefinition.getBeanClass().newInstance();
	}

	protected void applyPropertyValues(Object bean, BeanDefinition mbd) throws Exception {
		for (PropertyValue propertyValue : mbd.getPropertyValues().getPropertyValues()) {
			Field declaredField = bean.getClass().getDeclaredField(propertyValue.getName());
			declaredField.setAccessible(true);
			Object value = propertyValue.getValue();
			if (value instanceof BeanReference) {
				BeanReference beanReference = (BeanReference) value;
				value = getBean(beanReference.getName());
			}
			declaredField.set(bean, value);
		}
	}
}
	
	
	
	
	
为了解决循环依赖的问题，我们使用lazy-init的方式，将createBean的事情放到`getBean`的时候才执行，这样在注入bean的时候，
如果该属性对应的bean找不到，那么就先创建！因为总是先创建后注入，所以不会存在两个循环依赖的bean创建死锁的问题。
	








[Mukosame]:    http://sun035.github.io  "Mukosame"
