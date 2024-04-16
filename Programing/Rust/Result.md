enum that has an Ok variant and an Err variant.

has method .expect("REASON")
which when an Err value appears at runtime the program crashes and displays reason. the compiler sees the type after the expect method as the same type as the Ok type