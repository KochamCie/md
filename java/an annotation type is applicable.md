```java
/**
 * Indicates the contexts in which an annotation type is applicable. The
 * declaration contexts and type contexts in which an annotation type may be
 * applicable are specified in JLS 9.6.4.1, and denoted in source code by enum
 * constants of {@link ElementType java.lang.annotation.ElementType}.
 * 指示注释类型适用的上下文。 注释类型可能适用的声明上下文和类型上下文在JLS 9.6.4.1中指定，并以源代码通过枚举常数java.lang.annotation.ElementType表示 。 
 * <p>If an {@code @Target} meta-annotation is not present on an annotation type
 * {@code T} , then an annotation of type {@code T} may be written as a
 * modifier for any declaration except a type parameter declaration.
 *如果注释类型T上不存在@Target元T ，则可以将类型为T写为除了类型参数声明之外的任何声明的修饰符。 


 * <p>If an {@code @Target} meta-annotation is present, the compiler will enforce
 * the usage restrictions indicated by {@code ElementType}
 * enum constants, in line with JLS 9.7.4.
 *如果存在@Target元注释，则编译器将强制使用ElementType枚举常量指定的使用限制，符合JLS 9.7.4。 


 * <p>For example, this {@code @Target} meta-annotation indicates that the
 * declared type is itself a meta-annotation type.  It can only be used on
 * annotation type declarations:
 例如，此@Target元注释表示声明的类型本身是元注释类型。 它只能用于注释类型声明： 


 * <pre>
 *    &#064;Target(ElementType.ANNOTATION_TYPE)
 *    public &#064;interface MetaAnnotationType {
 *        ...
 *    }
 * </pre>
 *
 * <p>This {@code @Target} meta-annotation indicates that the declared type is
 * intended solely for use as a member type in complex annotation type
 * declarations.  It cannot be used to annotate anything directly:
 此@Target元注释表示声明的类型仅用于复杂注释类型声明中的成员类型。 它不能直接用于注释任何东西： 


 * <pre>
 *    &#064;Target({})
 *    public &#064;interface MemberType {
 *        ...
 *    }
 * </pre>
 *
 * <p>It is a compile-time error for a single {@code ElementType} constant to
 * appear more than once in an {@code @Target} annotation.  For example, the
 * following {@code @Target} meta-annotation is illegal:
 这是一个编译时错误ElementType不断出现不止一次在@Target注解。 例如，下面@Target元注释是非法的： 


 * <pre>
 *    &#064;Target({ElementType.FIELD, ElementType.METHOD, ElementType.FIELD})
 *    public &#064;interface Bogus {
 *        ...
 *    }
 * </pre>
 *
 * @since 1.5
 * @jls 9.6.4.1 @Target
 * @jls 9.7.4 Where Annotations May Appear
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Target {
    /**
     * Returns an array of the kinds of elements an annotation type
     * can be applied to.
     * @return an array of the kinds of elements an annotation type
     * can be applied to
     */
    ElementType[] value();
}
```